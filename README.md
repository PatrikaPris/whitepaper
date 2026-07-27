# Vegaswap whitepaper

Version 0.1.0

### Abstract

Vegaswap is a novel AMM protocol which is built as a platform to enable a wider range of DeFi applications. Dynamic pricing allows the protocol to adapt to market conditions. The economics for market makers and takers are designed to allow for long term viability. Issues of Impermenant Loss are prevented through the dynamic fee model. Governance of the marketplace happens through the Vega governance token.

### Marketplace


The marketplace enables traders to swap assets. The marketplace contract keeps track of the pools that have been created and are available for trading. Vegaswap allows for different types of pools to exist, which have different features depending on the assets traded and the governance of the pool.

 ```javascript
contract Marketplace {
  
	/// start a new pool which is called by delegates
	function StartDelegatedPool() RoleDelegate {

		FixedRatePool p = new FixedRatePool("Fixed");

		pools[poolnum] = address(p);

		poolnum++;

	}
  
	/// disolve a delegate pool
	function DissolveDelegatePool() RoleDelegate {

	}  

	/// start a pool, callable by anyone
	function StartPublicPool() public {
	// in version 1
	}

}
 ```

### Automated market-making

An automated market-maker (AMM) is a protocol to enable users to swap between any two blockchain based assets. This system of smart contracts prices assets at any given time in a formula. Uniswap has proven succesfully the implementation of a constant product marketmaker [1].

```javascript
/// the maker contract. Makers decide on prices given inputs
contract Maker {

	/// constant product MM. price an asset Y as a function of X, given the inventory of X and Y and delta x
	function pricing_CPMM(uint256 X, uint256 Y, uint256 dx) public returns (uint256) {
		uint256 K = X * Y;

		uint256 YD = K / (X + dx);

		uint256 dy = Y - YD;

		return dy;

	} 

}
```

In this formula the price is a relation of the available inventory. The price of the asset X in relation to Y is the function of its scarcity. The maker contract encapsulate the pricing mechanism.

Liquidity providers (LP's) supply capital in a pool of capital on an equal basis. The LP's own shares of the pool depending on the capital they provide. The simplicity of the protocol has proven itself in practise. It is important to note that LP's are not involved in any decision making besides adding and removing inventory.

### Options greeks and Vega

The well known derivatives for pricing options relate to the structure of a market and market-making in the following way. The name vegaswap relates to the option greek vega (ν). The option greeks [2] are the the deriviatves of prices with regards to variables:
 
Delta δ: change of the option value with respect to changes in the underlying asset's price.

Theta θ: the derivative of the option value with respect to to the passage of time.
 
Gamma Γ: the rate of change in the delta with respect to changes in the underlying price.

In the practise of Market-making volatility plays an important role. In a stable market i.e. one with low volatility the price of a directional bet is low.

Vega ν: the derivative of the option value with respect to the volatility of the underlying asset.

With higher volatility the risk of a market-maker increases and therefore he needs to increase the price of the bet. Vega is the derivative with regards to volatility and is a key parameter. Market-takers are long vega, as they want the price of an asset to move in their favour. Maker-makers are short vega, as they want to the price to be stable and earn the fees.

### Market making pools

The author has previously proposed a DAO on top of the 0x protocol [3]. This mechanism was proposed so that there is positive feedback loop to bootstrap liquidity for a DEX. It is different from a liquidity pool in that there are human operators of the marketmaking fund and there needs to be a profit and loss calculation. These pools would operate more like hedgefunds. In this model the market-making is completely general as the MM can be fullfil any function. This approach has the drawback of adding a third party, the trading operator. Vegaswap is the combination of the AMM protocol with the MM pool, as it allows a certain flexbility of the pricing mechanism, but with a equal basis accounting - LPs earn fees on a percentage basis and the Profit-loss calculation is trivial.

### AMM fees

Market-makers enable market-takers to speculate on the direction of the market. The price of the bet is the bid-ask spread. In uniswap the fees are statically encoded and not adaptive. This does not have to be the case. Dynamic fees would allow the market to adapt to changing dynamics as an orderbook pricing mechanism would. The fees are paid from the market takers i.e. participants taking directional positions to the liquidity providers i.e. the participants which supply capital are short vega.

### Impermanent loss and arbitrage

Pools earn fees, but can lose on arbitrage. If the underlying assets are trading on a more performant central exchange arbitrageurs can expolit the pool continously. This can be considered as an extra fee or loss for the traders. Therefore the fees need to be generally higher to componensate for this. This means that the profit and yield from supplying capital is lower than it could be. The goal of the Vega protocol is to reduce loss from arbitrage and maximize the potential for the owners of the pool and Vega token holders.

### Asset issuance pools

Trading of new tokens has fundamentally different charateristics than established ones. The volatility is much higher and the process of launching the market is different. Also for a new market, there is no arbitrage with Centralised exchanges, which makes the AMM the sole source of price discovery. In the case of a launch it is also important who creates the pool as the creators of the tokens will want to define the conditions under which the token is traded. (with a pool for ETH-USDC there is no concern who creates the pool initially). We call pools for asset issuance asset issuance pools (AIP) (see also [6]).

### Incentives and fee structure

The vega token represents ownership in all pools, i.e. the marketplace. They are acquired in the vega pool.

Supplies of capital earn a fair reward for being part of the pool and enabling transactions. In the uniswap and current AMM implementations the duration of the capital provision is immediate. LP's can remove their staked capital at any time. This has important consequences for pools of newly issued assets. In a highly liquid market it can be assumed that other LPs are always readily available to supply capital. In the case of asset issuance pools, the staked duration of the capital is important. Because usually a small group of individuals ("team") own the created tokens it is important to have a longer duration of the capital supplied ("rug pull"). Vegaswap allows the issuers to define higher rewards for the early LPs, so that the liquidity can be bootstrapped.

Users can earn rewards for trading and Liquidity providers (LP's) for supplying capital. LPs can act as marketers of their pool and earn rewards on a per bool basis. Vega has different pools and promotion of pools earns rewards.

### Staked value and profitability

A key metric for a defi application is the Total Value Locked (TVL). To enable bootstrapping rewards will be set higher initially and then decrease with TVL. The key metrics for rewards can be set by governance mechanisms, In order to harness the intelligence of the collective and make the system more decentralised.

### Multichain AMM

Current AMMs are deployed on a single chain and compete on a chain level (Pancake swap versus Uniswap). Vega is the first AMM built with Multichain in mind and using the best of each chain to improve the entire cross-chain ecosystem. Assets which don’t exist on the other chain require a pegging mechanism. Currently this can be only done through a central exchange, but Vega can enable decentral market makers and issuers.

![Alt](https://i.imgur.com/ZqJOLc1.png)

### Tokeneconomics

The tokeneconomics are designed to create an equitable distribution between investors, participants and liquidity prioviders.

Private sale (6.00%) - funding via SAFT agreement to bootstrap operations

Public sale (2.00%) - funding via IDO platforms

Trader program (3.00%) - incentives for traders to build and improve the platform

Development (7.00%) - smart contract and frontend development, R&D

Marketing (7.00%) - funds to improve marketing and exposure

Ecosystem (10.00%) - enable a wide ecosystem with projects building on vega

Vega Liquidity  (15.00%) - liquidity for Vega tokens

Liquidity grants (8.00%) - a grant program for Liquidity providers who benefit the ecosystem

Liquidity rewards (20.00%) - rewards for Liquidity providers

Treasury (2.00%) - initial funds in treasury.

Team  (15.00%) - incentives for core team. Community members are encourage to join core

Advisory (5.00%) - advisors to guide the project

![Alt](https://i.imgur.com/RDs7TVV.png)

### References

[1] https://uniswap.org/whitepaper.pdf

[2] [https://en.wikipedia.org/wiki/Greeks_(finance)](https://en.wikipedia.org/wiki/Greeks_(finance))

[3] https://forum.0x.org/t/marketmaker-incentives-mm-pool/341

[4] https://research.paradigm.xyz/uniswaps-alchemy

[5] Automated Market Makers for Decentralized Finance (DeFi), Yongge Wang, [https://arxiv.org/pdf/2009.01676.pdf](https://arxiv.org/pdf/2009.01676.pdf)

[6| https://medium.com/balancer-protocol/building-liquidity-into-token-distribution-a49d4286e0d4

[7] https://bitcointalk.org/index.php?topic=292769

[8] Xu, J., Vavryk, N., Paruch, K., & Cousaert, S. (2021). [SoK: Decentralized Exchanges (DEX) with Automated Market Maker (AMM) protocols](https://arxiv.org/abs/2103.12732). arXiv e-prints, arXiv-2103. [GitHub](https://github.com/xujiahuayz/bondingcurves/)
