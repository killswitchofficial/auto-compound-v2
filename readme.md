# PronteraReserve.sol
- KSW Token reserves for Prontera contract to withdraw

# PronteraV2.sol
- Masterchef that emits KSW Token.
- User can deposit, depositToken, depositEther to destination pools.
- User can withdraw, withdrawEther, emergencyWithdraw to remove assets from pools.
- Action that need swapping tokens before deposit (depositToken, depositEther) to destination pools, tokens will be sent to Juno contract and then validate amount out and deadline.
- Juno is proprietary of KillSwitch which will not be reveal. Juno is not related to Prontera.
- Prontera have functions for users to manage their shares (Jellopy represents shares of each asset). They are storeAllowance, approveStore, increaseStoreAllowance, decreaseStoreAllowance, depositFor, storeKeepJellopy, storeReturnJellopy, storeWithdraw. These functions will require users to approve and allow external contracts to manage their shares.

# Prontera approve flow
- approveStore functions are similar to ERC20 for 2 actions (To support take profit / stop loss features)
1. Keep Jellopy - is used to reserved jellopy amount from user.jellopy to user.storedJellopy (Users are unable to withdraw user.storedJellopy)
2. Withdraw Jellopy - is used to withdraw jellopy on behalf of user from user.storedJellopy (Pending KSW Token will directly send to user) (To return jellopy, store can call storeReturnJellopy)
- summary
user -> increaseStoreAllowance -> storeKeepJellopy -> storeWithdraw(optional)

# IzludeV2.sol
- a vault to receive token and calculate share of users before send tokens to strategy farm (Byalan)
- TVA = timelock 1 week (at least)

# PancakeByalanLP & PancakeByalanSingle.sol
- Farm strategy of Pancake LP and Pancake's Cake single pool
- Hydra (admin) is wallet address to pause, unpause, panic and setGasPrice

# AllocKafra.sol
- Used to validate, if user can deposit to farm and the condition is size of liquidity in destination farm.

# FeeKafra.sol
- Used to calculate fee when withdraw

# GasPrice.sol
- Used to limit gas price of harvest transactions

# Summary User flow
user -> deposit/withdraw -> pronteraV2
