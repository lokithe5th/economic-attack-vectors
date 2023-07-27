### 1. Market Makers  

A sale is when one asset is swapped for another asset. We swap our time (labour) for money. We swap our money for shares. And sometimes we swap the right to these shares for the privilige of borrowing other assets.  

Swapping assets which are limited in number (or scarce), and the study of the extensions and variations upon this idea, is micro-economics.  

To swap things we need a willing seller and a willing buyer. With this a market is made. 

Markets follow supply and demand. If the supply of an asset is low, and there are many buyers, then the buyers need to compete for the asset by offering a better price to the sellers. The opposite happens with an asset that is available in too great a quantity. The sellers now need to compete by accepting a lower price to buyers.

Price discovery happens as the prices being offered and accepted converge. Given a fixed supply and a fixed demand the price will at some point in time become more balanced. We know that supply and demand is not fixed. And so price varies as the market receives more information and reacts to that information (for example XRP before and after July 2023).  

#### Order books  
Markets is where buyers and sellers meet. In TradFi we use order books to match the buyers with sellers.

A buyer will open an order for 100 asset X in exchange for 10 asset Y. The seller can only sell if there is someone else taking the buy side. The buyer can only purchase asset Y if there is someone else who wants it at that price.  

Order books are also used in centralized exchanges (CEXes), both in TradFi and DeFi.  

Depending on the asset, orders can take some time to fill. They may also fail. 

As the market is not static, it price may move in either parties favor. 

>>> Swapping is a zero-sum game, and so, if one party is favored the other party is at a disadvantage. This is fundamental to all markets.

#### AMMs  
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

The reserves of `x` and `y` are provided by users who are not using their `x` and `y` assets at the moment. By providing liquidity they create a pool of assets in the smart contract. Other users can then swap asset `x` for `y` at the rate determined by `k = xy`. Price discovery happens over time while swaps trend in one direction. But the initial supply of liquidity to the pool determins the swap price. The liquidity providers are rewarded for providing their assets by receiving fees from the swaps. This is similar to what banks do with customer money sitting in savings accounts. They take risks (trades, swaps, investments) with it and then reward you for providing capital with interest.  

This is a primitive in DeFi.  

The design and implementation of an AMM can vary widely. The two most common are constant product AMMs and constant sum AMMs.

##### Thoughts for security researchers  
From a security perspective we see that such a design allows certain actions:  

**If the reserves of a smart contract can be added to assymmetrically, the price will change.**  

This is intended, as swaps in one direction is not separate from large deposits in one direction. In both cases the reserves of an asset are altered and the price will move in favor of buyers for the asset being supplied and vice versa (see the above on supply and demand).  

After spending many hours trying to trick AMMs into giving better prices the following holds true:  
1. In most cases, it's rarely a valid issue if the reserves/price can be manipulated by a user. The attacker needs to spend resources to accoplish the manipulation, and the implementation of the AMMs serve to limit the benefit from this. In most cases they will lose assets, even if they can manipulate the price. 
2. Vulnerabilities can arise when the price in the AMM is used in other places in the protocol. Always identify which new code *assumes* an unmanipulated price.
3. Custom logic in `routers` and `pair` contracts often hide vulnerabilities.

#### Constant Product AMMs  

#### Constant Sum AMMs  

#### Exploits

##### Price Manipulation  

##### Poorly designed AMMs  

##### Front-running  
