## 2. Lending  

Lending is as integral to DeFi as it is to TradFi.  

The motivation in DeFi is much the same as in TradFi. Users supply assets into a protocol, and other users are charged interest, or fees, to borrow these assets.  

Lending can be decentralized because users have to provide more collateral than they borrow.  

How can this be useful to a user? By depositing assets into a protocol the depositor is able to maintain exposure to that asset, while still being able to borrow a less volatile asset to conduct business in.  

The concept of leverage is fundamental to finance, and this is one of the primitives required for this to function.  

### Leverage  

> We assume a very simplistic lending model with no penalty fees.

BOB has 100 ether, which he deposits into a lending protocol. He is able to borrow about 80% of his collateral value in DAI (assuming the collateral factor is 0.8). 

For simplicity's sake we assume 1 ether has a price of 1000 DAI. BOB now withdraws 80 000 DAI from the lending protocol.  

What does he do? He goes degen, and he swaps the 80k DAI for 80 ether. Then he deposits it back into the protocol. He has now deposited 180 ether into the protocol. His collateral factor (loan to asset ratio) is now well below 80%.  

He now borrows 64k DAI more, to make use of his healthy collateral factor, swaps for ether and deposits it into the protocol again. 

*Why would someone do this?*  
BOB believes the price of ether will go up. He is expressing this sentiment by taking a leveraged position. After depositing the last 64 ether, BOB has grown his initial 100 ether position to about 244 ether.  

If the ether price moves upwards, he will make a profit.  

He owes, at this point, 144k DAI (80k + 64k). He has deposited into the protocol a total of 244 ether (100 + 80 + 64), worth 244k DAI. If the price were to move up by 20%, his deposit would be worth 292.8k DAI. He can now withdraw 120 ether, swap into DAI and repay his 144k DAI debt. This leaves him with 124 ether, 24 ether more than he started with.  

> The price of ether only changed by 20% but BOB made a return of 24%. This is the power of leverage.  

The example is simply one strategy that is made possible with lending markets in finance.  

*What were to happen if the price of ether dropped?*  

#### Liquidation  

A lending protocol requires BOB's liquidation collateral factor, i.e. his loan to asset ratio, to remain above a certain amount. For our example this is equal to the borrow collateral factor, i.e. 80%.  

BOB had liabilities of 144k DAI, and assets of 244 ether. At the starting price of 1000 DAI / ETH his collateral factor was 0.59. This seems like a healthy amount. 

If the price of ether dropped 30% to 700 DAI, then his collateral factor becomes 0.84, which is above the liquidation threshold. 

> At the liquidation threshold a user's assets are no longer deemed acceptable collateral for their debts.  

ALICE sees that BOB's collateral value has weakened and liquidates his position. If BOB had simply supplied 244 ether and borrowed 144k DAI, then it would mean:  
- He has 144k DAI in his wallet. 
- His 244 ether is sold off for about 170.8k DAI (244 ether @ 700 DAI). 
- BOB's account is then credited with the 170.8k DAI, meaning his debt is wiped clean and is credited with 26.8k DAI worth of ether (38.29 ether). 
- In this scenario, where BOB supplied 244 ether and borrowed 144k DAI, he ends with 38.29 ether and 144k DAI, worth a total 170.8k DAI.

Unfortunately, in our case BOB has used leverage. His initial 100 ether was used to borrow, swap into ETH and then deposit back into the protocol. This means he has 0 DAI in his wallet, and 244 ether in the lending protocol. His collateral is sold off for 170.8k DAI, which is used to wipe his debt clean. He is still credited with the 38.29 ether, meaning his total asset value is now 26.8k DAI. A loss of 73.2k DAI (remember he started with 100k DAI worth of ether). Compared to the loss of 30k DAI he would have had for simply holding ether.  

> Leverage is dangerous for this reason. Price movements, i.e. profits and losses will be magnified.  

Liquidations are crucial for a healthy lending market. It prevents lending pools from holding collateral assets that no longer cover the cost of the assets lent to other users. I.e. it prevents protocol insolvency.  

In our example, BOB still received some ether back from the protocol. Because the collateral he supplied covered the total value of his debt.  

What would happen if the price of ether suddenly fell 60%? If the protocol sold off his 244 ether it would only receive 97.6k DAI, which does not cover the 144k BOB borrowed from the protocol. The protocol has incurred bad debt, and a liquidation in this case would mean BOB receives none of his collateral back and the protocol has lost about 43k DAI of value. BOB has lost his shirt.

#### Thoughts for security researchers  

1. From the above we can determine that the borrow and liquidation collateral factors (or a protocol's variation on this) is of paramount importance. If an attacker can manipulate these factors the protocol may leak value.  
2. The collateral factors are based on asset price. `PriceFeeds` are crucial to a lending market. 