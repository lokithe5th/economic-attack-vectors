## 1. Market Makers  

A sale is when one asset is swapped for another asset. We swap our time (labour) for money. We swap our money for shares. And sometimes we swap the right to these shares for the privilige of borrowing other assets.  

Swapping assets which are limited in number (i.e. scarce), and the study of the extensions and variations upon this idea, is micro-economics.  

To swap things we need a willing seller and a willing buyer. With this a market is made. 

Markets follow supply and demand. If the supply of an asset is low, and there are many buyers, then the buyers need to compete for the asset by offering a better price to the sellers. The opposite happens with an asset that is available in too great a quantity. The sellers now need to compete by accepting a lower price to buyers.

Price discovery happens as the prices being offered and accepted converge. Given a fixed supply and a fixed demand the price will at some point in time become more balanced. We know that supply and demand is not fixed. And so price varies as the market receives more information and reacts to that information (for example XRP before and after July 2023).  

### Order books  
Markets is where buyers and sellers meet. In TradFi we use order books to match the buyers with sellers.

A buyer will open an order for 100 asset X in exchange for 10 asset Y. The seller can only sell if there is someone else taking the buy side. The buyer can only purchase asset Y if there is someone else who wants to sell it at that price.  

Order books are also used in centralized exchanges (CEXes), both in TradFi and DeFi.  

Depending on the asset, orders can take some time to fill. They may also fail. 

As the market is not static, a price may move in either party's favor. 

> Swapping is a zero-sum game, and so, if one party is favored the other party is at a disadvantage. This is fundamental to all markets.

### AMMs  
An automated market maker (AMM) means that a smart contract can play the role of willing buyer or willing seller in any swap transaction. This eliminates the issue of making a market (spoiler alert: it creates a different issue).  

An AMM uses an invariant to determine the current swap price for the assets in the pool.  

For example:

```
k = xy
``` 

Where: 
`x` = the reserves of token X
`y` = the reserves of token Y
`k` = the invariant  

[There is a highly recommended article about AMMs here](https://medium.com/gammaswap-labs/uniswap-v2-a-constant-function-market-maker-78317ea6d4ac)  

The reserves of `x` and `y` are provided by users who are not using their `x` and `y` assets at the moment. By providing liquidity they create a pool of assets in the smart contract. Other users can then swap asset `x` for `y` at the rate determined by `k = xy`. Price discovery happens over time when swaps trend in one direction. But the initial supply of liquidity to the pool determines the swap price. The liquidity providers are rewarded for providing their assets by receiving fees from the swaps. This is similar to what banks do with customer money sitting in savings accounts. They take risks (trades, swaps, investments) with it and then reward you for providing capital with interest.  

This is a primitive in DeFi.  

The design and implementation of an AMM can vary widely. The two most common are constant product AMMs and constant sum AMMs. [There is a brilliant article about AMMs here](https://link.springer.com/article/10.1186/s40854-021-00314-5)

![Figure from: Mohan, V. Automated market makers and decentralized exchanges: a DeFi primer. Financ Innov 8, 20 (2022). https://doi.org/10.1186/s40854-021-00314-5](https://link.springer.com/article/10.1186/s40854-021-00314-5/figures/1)

### Constant Product AMMs  
A common example of a CPMM is classic Uniswap. The [V2](https://github.com/Uniswap/v2-core/blob/master/contracts/UniswapV2Pair.sol) Pair (the pool where the tokens are held for swaps) code looks like this:  

```
 function swap(uint amount0Out, uint amount1Out, address to, bytes calldata data) external lock {
        require(amount0Out > 0 || amount1Out > 0, 'UniswapV2: INSUFFICIENT_OUTPUT_AMOUNT');
        (uint112 _reserve0, uint112 _reserve1,) = getReserves(); // gas savings
        require(amount0Out < _reserve0 && amount1Out < _reserve1, 'UniswapV2: INSUFFICIENT_LIQUIDITY');

        uint balance0;
        uint balance1;
        { // scope for _token{0,1}, avoids stack too deep errors
        address _token0 = token0;
        address _token1 = token1;
        require(to != _token0 && to != _token1, 'UniswapV2: INVALID_TO');
        if (amount0Out > 0) _safeTransfer(_token0, to, amount0Out); // optimistically transfer tokens
        if (amount1Out > 0) _safeTransfer(_token1, to, amount1Out); // optimistically transfer tokens
        if (data.length > 0) IUniswapV2Callee(to).uniswapV2Call(msg.sender, amount0Out, amount1Out, data);
        balance0 = IERC20(_token0).balanceOf(address(this));
        balance1 = IERC20(_token1).balanceOf(address(this));
        }
        uint amount0In = balance0 > _reserve0 - amount0Out ? balance0 - (_reserve0 - amount0Out) : 0;
        uint amount1In = balance1 > _reserve1 - amount1Out ? balance1 - (_reserve1 - amount1Out) : 0;
        require(amount0In > 0 || amount1In > 0, 'UniswapV2: INSUFFICIENT_INPUT_AMOUNT');
        { // scope for reserve{0,1}Adjusted, avoids stack too deep errors
        uint balance0Adjusted = balance0.mul(1000).sub(amount0In.mul(3));
        uint balance1Adjusted = balance1.mul(1000).sub(amount1In.mul(3));
        require(balance0Adjusted.mul(balance1Adjusted) >= uint(_reserve0).mul(_reserve1).mul(1000**2), 'UniswapV2: K');
        }

        _update(balance0, balance1, _reserve0, _reserve1);
        emit Swap(msg.sender, amount0In, amount1In, amount0Out, amount1Out, to);
    }
```

A usual design will be a Router contract which should call the `swap` function in the pair contract. Users interacting with the pair contract directly risk losing funds as the intended access point is a router contract. An example of a `router` can be found in the [V2 periphery repo](https://github.com/Uniswap/v2-periphery/blob/master/contracts/UniswapV2Router02.sol):  

```
    function swapExactTokensForTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external override ensure(deadline) returns (uint[] memory amounts) {
        amounts = UniswapV2Library.getAmountsOut(factory, amountIn, path);
        require(amounts[amounts.length - 1] >= amountOutMin, 'UniswapV2Router: INSUFFICIENT_OUTPUT_AMOUNT');
        TransferHelper.safeTransferFrom(path[0], msg.sender, UniswapV2Library.pairFor(factory, path[0], path[1]), amounts[0]);
        _swap(amounts, path, to);
    }
```

Can you see where the check for the invariant is?  

It is located in the `pair` contract in this line:  
`require(balance0Adjusted.mul(balance1Adjusted) >= uint(_reserve0).mul(_reserve1).mul(1000**2), 'UniswapV2: K');`  

The expression is that we require the following to hold true: `balance0Adjusted * balance1Adjusted >= reserve0 * reserve1`. The manipulation of the reserves by adding asymmetrically is explicitly accepted, as `k = reserve0 * reserve1`, and the code enforces that the new balances (excluding newly transferred funds) continues to hold *at least* the `k`. 

##### Thoughts for security researchers  
From a security perspective we see that such a design allows certain actions:  

**If the reserves of a smart contract can be added to assymmetrically, the price will change.**  

This is intended, as swaps in one direction is not separate from large deposits in one direction. In both cases the reserves of an asset are altered and the price will move in favor of buyers for the asset being supplied and vice versa (see the above on supply and demand).  

From experience, after spending many hours of bug hunting trying to trick AMMs into giving better prices the following holds true:  
1. In most cases, it's rarely a valid issue if the reserves/price can be manipulated by a user. The attacker needs to spend resources to accoplish the manipulation, and the implementation of the AMMs serve to limit the benefit from this. In most cases they will lose assets, even if they can manipulate the price. If you can prove the contrary, you most likely have a valid vulnerability.
2. Vulnerabilities can arise when the price in the AMM is used in other places in the protocol. Always identify which new code *assumes* an unmanipulated price.
3. Custom logic in `routers` and `pair` contracts often hide vulnerabilities.

### Constant Sum AMMs  

### Exploits

#### Price Manipulation  

#### Poorly designed AMMs  

#### Front-running  
