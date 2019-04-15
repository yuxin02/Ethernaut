# Level08 Vault

[A Decompiler for Blackhain-Based Smart Contracts Bytecode](https://www.slideshare.net/Shakacon/a-decompiler-for-blackhainbased-smart-contracts-bytecode)


区块链上所有东西都是公开的，查看json rpc文档可知道有eth.getStorageAt() API，根据solidity的“对象模型” ( http://solidity.readthedocs.io/en/develop/miscellaneous.html )，可知password放在storage的1的位置

### 方法一
console里输入即可查看密码
```
web3.eth.getStorageAt(contract.address, 1, console.log);
```

根据实验结果， 这个没有任何输出，why？？？

### 方法二
通过infura提供的节点调用，需要先注册获取token

```js
> var Web3 = require('web3');
> var web3 = new Web3(new Web3.providers.HttpProvider("https://ropsten.infura.io/<mytoken>"));
> console.log(web3.currentProvider);
> let vaultAddress = instance // contract.address
> web3.eth.getStorageAt(vaultAddress,1)
> console.log('ASCII: '+ web3.toAscii(web3.eth.getStorageAt(vaultAddress,1)))
ASCII: A very strong secret password :)
```


根据得到32bytes密码: `A very strong secret password :)`，在console中`contract.unlokc('A very strong secret password :)')`

参考：
[Solving the Ethernaut Coin Flip, Telephone & Vault challenges (solidity)](https://medium.com/@MisterCh0c/solving-the-ethernaut-coin-flip-telephone-vault-challenges-solidity-41d8659034b4)

[zkSNARKs in a nutshell](https://blog.ethereum.org/2016/12/05/zksnarks-in-a-nutshell/)