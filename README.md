# blockchain-hw1
## Задание 1
https://github.com/gnosis/MultiSigWallet/blob/master/contracts/MultiSigWallet.sol - сделать, чтобы с баланса multisig-контракта за одну транзакцию не могло бы уйти больше, чем 66 ETH

```
- 94:       /// @dev Fallback function allows to deposit ether.
+ 94:       modifier validTransactionValue(uint value) {
+ 95:           require(value <= 66 ether); // want to have a message like "value cannot be greater than 66 ETH" here, but cannot due to version -- it's supported since Solidity 0.4.22, but then there are lots of `deprecated` errors in this code, so nevermind :(
+ 96:           _;
+ 97:       }
+ 98:       
+ 99:       /// @dev Fallback function allows to deposit ether.

- 191:      returns (uint transactionId)
+ 191:      validTransactionValue(value)
            returns (uint transactionId)
```
