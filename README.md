# BIP
Solidity
Questions:
1. payable. 
因为代码在Remix 上运行提示errored: VM error: revert.
revert	The transaction has been reverted to the initial state.
Note: The constructor should be payable if you send value.
所以我在创建了constructor 并添加了payable modifier。但是对于 payable, 我在Solidity 官方doc上找到的说明是，payable关键字允许function被调用的时候接受以太币。那如果我自己定义了个数组，通过function调用数组来改变数组里的值这种情况是否就无需payable？(因为我想定义个user的struct，这样相当于每个用户的账户值就是数组里中的某个值)。

2. Communicator
因为想用某个确定地址的communicator去与sender或者reciever去交流(只有communicator之间互相知道才能通过地址互相交流，所以需要确定地址)，所以在程序里设置了2个地址。但是如果我按照下面的方式去new一个communicator，我该如何得到这个地址？
             address cAddr = 0x72bA7d8E73Fe8Eb666Ea66babC8116a41bFb10e2;
             CommunicatorSend cSender = new CommunicatorSend(cAddr);
不用new，只是CommunicatorSend cSender = CommunicatorSend(cAddr) 的话，就又遇到之前的error VM error: revert.
不清楚为什么出现这个问题。

3. msg.sender
本来是想通过msg.sender来调取该地址的amount，但是在remix debug该如何引入msg.sender? 因为不知道该如果给入这个msg。sender我目前是直接在function里面给address这个入参，然后通过比较该地址的amount是否大于要转出的额度进行判断。

4.
通过sender call 了communicatorSend的requestSend function，而且也能把要转出的balance先转到sender里。
在这个function中调用communicatorReceive中的requestReceive function(把balance转到receiver) 这步出现错误。显示：
0x0 Transaction mined but execution failed. (我现在在尝试判断是否因为gas 导致这个问题)
想请教大家该如何解决。

5. contracts[b].call.value(balance)()
我能理解是通过call把balance加到contracts[b]这个地址。但是一直很困恼不知道这个balance是谁给付出的。
以及这个address.balance 是直接加到address上，和我之前设置的struct里的amount应该不是同一个东西？

6.
在function onSend(uint balance, address a, bool isSuccess) public{
        if(isSuccess){
            bool flag = a.call("onSend", balance, a, true);
            if(flag)
            {
                managedContracts[a] = managedContracts[a] - balance;
            }
        }
    }
中analysis提示
Potential Violation of Checks-Effects-Interaction pattern in CommunicatorSend.onSend(uint256,address,bool): Could potentially lead to re-entrancy vulnerability. 
