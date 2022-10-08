## PANCAKE SMART CONTRACT [SyrupBar.sol] REVIEW

PancakeSwap review in generally, PancakeSwap is one of the most popular decentralized exchanges (DEXs) on Binance Smart Chain that employs an automated market maker mechanism to facilitate the secure BEP-20 token swaps. 

PancakeSwap is a decentralized exchange (DEX) on the Binance Smart Chain (BSC) that uses an automated market maker mechanism to make token swaps possible. It was released in September 2020 by a group of anonymous developers. PancakeSwap is also a permissionless DEX, allowing anyone to list their tokens on the exchange as long as they build a liquidity pool for the token.

PancakeSwap provides its users with a wide range of features like token swaps, liquidity provision & farming, perpetual trading, staking, lottery, NFT marketplace, launchpad, etc.


### The SyrupBar.sol explaination -

SyrupBar is a governance token that the master chief contract uses. The contract contain mainly two part which are the `token` part and the `governance` part. 


```solidity
  import "bsc-library/contracts/BEP20.sol";
  import "./CakeToken.sol";
```

The first (1) `import` which is bsc-library is a contract token like ERC20, but this one is BEP20 - meaning that is on the Binance Smart Chain which execute the BEP20 standard for a token.

The second (2) `import` which is CakeToken.sol - on PancakeSwap it makes it leverages its own utility token known as CAKE, which is the PancakeSwap token. The CAKE token is used in a variety of different ways within the PancakeSwap platform, including: Yield Farming – done through the PancakeSwap farm. PancakeSwap staking.

```solidity
   contract SyrupBar is BEP20("SyrupBar Token", "SYRUP") {
```

This contract inheriting the contract BEP20, this is the contract name `SYRUP`. This include that the contract inherit all function, also the constructor in the BEP20 inherited takes in two parameter which are the `name` and `symbol` of the token.

## This is the contructor function

```solidity
     constructor(CakeToken _cake) public {
        cake = _cake;
    }
```

constructor: the contructor function taken the parameter `_cake` and it set the address of the cake contract in the state variable, the function only runs once when it is deployed.

## Functions 

```solidity
   function mint(address _to, uint256 _amount) public onlyOwner {
        _mint(_to, _amount);
        _moveDelegates(address(0), _delegates[_to], _amount);
    }
```
mint: The `_mint` function is called inside this function to mint token to the to address and the inputed `_amount` of token is the amount of to be minted to the `_to` address. Note, that this function can only be called by the `msg.sender` specified in the `onlyOwner` modifier which happen to be the master chief.


```solidity
    function burn(address _from, uint256 _amount) public onlyOwner {
        _burn(_from, _amount);
        _moveDelegates(_delegates[_from], address(0), _amount);
    }
```

burn: The _burn function from the BEP20 contract inherited. It burns Syrupbar token. In this case, it can only be called by the owner -- which has to be the master chief of the onlyOwner modifier.

```solidity
  function safeCakeTransfer(address _to, uint256 _amount) public onlyOwner {
        uint256 cakeBal = cake.balanceOf(address(this));
        if (_amount > cakeBal) {
            cake.transfer(_to, cakeBal);
        } else {
            cake.transfer(_to, _amount);
   }
```

safeCakeTransfer: The safecakeTransfer actually transfer to the `address _to` that is stated in the parameter, using the amount in the parameter. A condition was set in .. to avoid the arounding error of not having a enough cake in the contract. Therefore, if the amount pass in is greater than the total balance of the cake token in this contract. it transfer the total balance of the cake token in the contract, else it transfer the amount stated in the parameter.

```solidity
   function delegates(address delegator) external view returns (address) {
        return _delegates[delegator];
    }
```

delegates: This function return the account of the delegate. The `_delegate` mapping name takes in the delegator pass in a parameter to return the account of the delegate. 






