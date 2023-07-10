# Xion Finance Smart Contracts

The Xion Finance smart contracts running on the Polygon chain, powering the Xion Finance one-click to DeFi ecosystem.

## Smart Contracts

### Xion Global Token <img src="https://xion.finance/images/xgt_icon.png" width="16" height="16"> XGT

The Xion Global Token (XGT) is a standard ERC20 token based on the OpenZeppelin contracts. We added the following features on top of it:

- Protection against sending tokens to the token address itself, e.g.:
- `require(recipient != address(this), "XGT-CANT-TRANSFER-TO-CONTRACT");`
- Blacklisting Bots
- Whitelisting Pools for Tax
- XGT Buy & Freeze (Prototype)
- Upgradeable Functions for Future-Proofing

You can find the contract [here](https://github.com/Xion-Finance/xion-finance-smart-contracts-v2/blob/master/contracts/token/XGTTokenV3.sol).

### XGT Bridge

In order to allow users to use their <img src="https://xion.finance/images/xgt_icon.png" width="12" height="12"> XGT on any of these chains above, we created a custom <img src="https://xion.finance/images/xgt_icon.png" width="12" height="12"> XGT cross chain bridge based on xDai's [Arbitrary Message Bridge](https://docs.tokenbridge.net/eth-xdai-amb-bridge/about-the-eth-xdai-amb). Users can freely and without any fee (besides the gas) send their <img src="https://xion.finance/images/xgt_icon.png" width="12" height="12"> XGT between xDai and Binance Smart Chain as well as between xDai and Ethereum Mainnet.

### XGT Reward Chest

In order to reward our users, we developed a custom reward chest contract, handling the rewarding of users with <img src="https://xion.finance/images/xgt_icon.png" width="12" height="12"> XGT, whether it is through earning, farming or cash-backs. This allows us to reward liquidity providers of pools that are not ours (such as the Pancake Swap and Honeyswap Pools) with <img src="https://xion.finance/images/xgt_icon.png" width="12" height="12"> XGT.

The contract itself only provides the base functionality, while the individual modules are providing the specific features.

#### Earning (Staking Module)

We recently added a way for users to stake their <img src="https://xion.finance/images/xgt_icon.png" width="12" height="12"> XGT in order earn compounded interest throughout the year. Currently users are earning a 150% APY, both on the xDai chain and the Binance Smart Chain.

We incorporated this functionality directly into our reward chest module system, so the earning part is implemented in the form of a StakingModule contract. Users can call the `deposit()` and `withdraw()` functions at any time, only incurring a small withdraw penalty if they withdraw prior to 72 hours after their last deposit. Deposits of merchants that are auto-staking their sale-earnings through the staking module will not count towards that penalty, so they can withdraw their staked XGT without a penalty at all times.

The contract is built in a way so the rewards auto-compound each time someone interacts with the contract. This way, our users will receive even higher APYs!

#### Farming (Pool Module)

As soon as a user is involved in a transfer of Pool tokens (either through minting, burning, or trading them), our backend node picks this up and calls the <img src="https://xion.finance/images/xgt_icon.png" width="12" height="12"> XGT Reward Chest contract to indicate that a certain user needs to be updated. The corresponding function works in a trustless manner, such that the contract itself verifies how many <img src="https://xion.finance/images/xgt_icon.png" width="12" height="12"> XGT LP tokens the user has. Based on this, the reward distribution of <img src="https://xion.finance/images/xgt_icon.png" width="12" height="12"> XGT starts (or ends).

#### Cashbacks (Cashback Module)

In one of our last updates we also added the Cashback Module, allowing merchants and the platform to issue cashbacks to customers. These cashbacks are also implemented with the possibility of mini-vestings, where the cashback amount is distributed to the customers over a period of several days, weeks or months.

Cashbacks can directly be claimed via the general `claim()` function of the reward chest through our UI.

#### Airdrops (Airdrop Module)

With our new airdrop module, we are able to easily distribute regular as well as vested airdrops to our users. The resulting <img src="https://xion.finance/images/xgt_icon.png" width="12" height="12"> XGT can be claimed via the Reward Chest like any other reward.

### Vesting

In order to incentivize the team and investors long-term, we are making use of a standard vesting contracts, that distributes the allocated tokens over a predefined vesting schedule.

### Upgradeability

We are leveraging the Upgradeability features by OpenZeppelin, allowing us to introduce features without changing the contract's address as well as fixing any unforeseen bugs that could lead to a financial loss for our users. The safety of our users and consequently their funds is of utmost importance to us!
However, we are **not** using this feature for the bridges and token itself, in order to maintain decentralization and not even having the possibility to gain access to our users funds.

## License

[GNU Affero General Public License v3.0](https://github.com/xion-global/xionfinance_smartcontract/blob/master/LICENSE)
