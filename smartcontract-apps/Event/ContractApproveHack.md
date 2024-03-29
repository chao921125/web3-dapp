# 又是用户转移资产权限被盗，如何确保加密资产安全？

2022 年 10 月 2 日凌晨 4 点，TP 钱包旗下闪兑协议 Transit Swap 用户转移资产权限被盗，目前合约上用户资产权限被盗导致的损失已经超过 1500 万美元。

## 什么是用户资产转移权限

在符合 ERC20 标准的合约中，都要实现函数 `approve` 、`allowance` 和 `transferFrom`。其中 `approve` 允许用户授权某个合约转移自己一定数量的资产，`allowance` 可以查询某个合约可以转移自己多少资产, 而 `transferFrom` 则允许被授权合约转移用户授权数量的资产。

## 为什么要有用户资产转移权限

如果想让用户向一个合约地址发送资产并调用该合约，那么在 ERC20 标准下，就只能有两种实现：一种是先发送一笔向合约转账资产的交易，再发送一笔调用合约的交易；另一种则是先发送一笔向合约授权用户资产转移权限的交易，再发送一笔调用合约的交易。而第一种方式在实现合约功能时会有很多问题，所以几乎所有的合约应用都选择第二种方式来让用户进行交互。

## 为什么授权用户资产转移权限会有安全风险

为了应用使用方便、节省 GAS 和减少系统复杂度，大多数的合约应用在让用户授权资产转移权限时都默认 `unlimited`, 就是无限数量授权。如果合约是开源并且有严格权限管理的，那么在理论上也不会有什么问题。但实际情况下会有很多的合约应用既没有开源也没有做好权限管理，而用户对此也是一无所知，为了能用就稀里糊涂地把自己资产全部授权了出去。

## 合约上用户资产转移权限被盗则么办

1. 首先你授权出去的资产就已经不完全是自己的了，如果合约还出现什么风险，一定要第一时间去解除该合约的用户资产转移权限（这里推荐 https://revoke.cash/）。
2. 如果风险合约在多链上部署（特别是跨链桥），那么一定要在自己使用过该应用的所有链上都去解除一遍用户资产转移权限。
3. 面对这种攻击，使用硬件钱包并不会更加安全，一定也要去解除该合约的用户资产转移权限。
4. 授权太多一时间清理不完，可以把资产转账到一个没有授权的的安全钱包。

## 如何减少授权用户资产转移权限带来的安全风险

1. 授权时不要使用无限数量授权、使用完就解除授权，或者定期清理授权。
2. 少用主流之外的合约应用，特别是不开源的合约应用。
3. 授权多的钱包里不要放太多钱，或者全部换成原生代币（比如 ETH 链上的 ETH，BSC 链上的 BNB）。
4. 以上都是用户层面的考虑，但想要在根本上解决这个问题，就必须在协议层面推出更加安全的资产合约标准，并在开发者群体内广泛应用（但受限于历史和合约不可修改的特性，这很困难）。
