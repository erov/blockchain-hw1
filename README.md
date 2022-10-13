# blockchain-hw1
## Задание 1
> https://github.com/gnosis/MultiSigWallet/blob/master/contracts/MultiSigWallet.sol - сделать, чтобы с баланса multisig-контракта за одну транзакцию не могло бы уйти больше, чем 66 ETH

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

## Задание 2
> https://github.com/OpenZeppelin/openzeppelin-contracts/blob/f2112be4d8e2b8798f789b948f2a7625b2350fe7/contracts/token/ERC20/ERC20.sol - сделать, чтобы токен не мог бы быть transferred по субботам

Насколько я помню из курса лекций, в вопросах, касающихся времени мы не можем оперировать 'глобальным' временем, единственное, что нам досупно -- block.timestamp, который по сути может быть любым значением на усмотрение майнера, но давайте решим задачу в предположении что там хранится условное JavaScript'овое `new Date()`.
Хотя по-прежнему у меня остаются вопросы касательно работы с високосными годами и фактом, что не во всех сутках в году ровно 24 часа, попробую закрыть на это глаза. Поскольку block.timestamp это таймстемп относительно Unix epoch (01.01.1970, thursday), следующий код с поправкой на вышесказанное, должно быть, запрещает транзакции быть рассчитанной, а как следствие запрещает токену быть transferred, в субботу:
```
- 211:      
+ 211:      uint32 dayInWeek = uint32(block.timestamp % 1 weeks); // trying in 'gas economy' 
+ 212:      require(2 days <= dayInWeek && dayInWeek < 3 days, "Cannot execute token transaction in unix epoch Saturday");
+ 213:      
```
