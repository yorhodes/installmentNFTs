## NFT Installments on [Dharma](https://dharma.io)

### Detailed Flow Diagram:

![](https://i.imgur.com/6A3eLVg.png)

### Flow Narration: 
1. Buyer identifies a desirable NFT listed for sale.

2. Buyer has an intent to borrow, and becomes a debtor in the context of Dharma. 

3. Buyer (debtor) signs `customDebtOrder` and instantiates on relayer.

4. Lendor with liquidity identifies a desirable `customDebtOrder` listed for investment.

5. Lendor instantiates a `customProxy` and provides it with funding for NFT purchase as specified in `customDebtOrder`.

6. `customProxy` signs and fills `customDebtOrder`, becoming a creditor in the context of Dharma.

7. Dharma [DebtKernel](https://github.com/dharmaprotocol/charta/blob/master/contracts/DebtKernel.sol) initializes loan upon fill by invoking relevant `registerTermStart`.

8. `customProxy` purchases NFT and receives [DebtToken](https://developer.dharma.io/primers/debt-tokens) upon loan init, forwarding NFT to be collateralized and DebtToken to lendor.

9. Dharma DebtKernel manages life cycle of loan using terms specified in `customDebtOrder`. 

## References

### `customDebtOrder`

- `principal = 0`
- `fees = 0` (no underwriter since the NFT is fully collateralized, relayer free)
- address of `customTermsContract` template
- params for `customTermsContract`:
    - NFT `sale address`
    - NFT `price`
    - `interest rate`
    - `duration`

### `customTermsContract`

- `registerTermStart`
    - invokes creditor `purchaseNFT`
    - retrieves NFT from creditor
    - collateralizes NFT using `customERC721Collateralizer`
    - reverts on failure
-  taken from [ERC721CollateralizedSimpleInterestTermsContract](https://github.com/dharmaprotocol/charta/blob/master/contracts/examples/ERC721CollateralizedSimpleInterestTermsContract.sol):
    - `registerRepayment`
    - `getExpectedRepaymentValue`
    - `getValueRepaidToDate`
    - `getTermEndTimestamp` 

### `customERC721Collateralizer`

- `collateralize`
    - transfer NFT from creditor instead of debtor
- taken from [ERC721Collateralizer](https://github.com/dharmaprotocol/charta/blob/master/contracts/ERC721Collateralizer.sol):
    - `seizeCollateral`
    - `returnCollateral`

### `customProxy`

- `constructor`
    - gives proxy funding from Lendor
    - sets `owner` to Lendor
- `purchaseNFT`
    - purchases NFT with funding
    - reverts on failure
- `retrieveDebtToken`
    - allows `owner` to extract DebtToken from proxy after loan init
- `retrieveCollateral`
    - allows `owner` to extract NFT from proxy after loan default

### UI Interfaces:

The borrower interface for browsing NFTs and instantiating intent to borrow could very easily be implemented as a wrapper around the [OpenSea marketplace](https://opensea.io/assets). Initially, though, we will build a very simple interface for instantiating `DebtOrder`s using our `customTermsContract` and inputting relevant parameters, to be listed on our relayer/lendor interface.

The lendor interface for browsing `DebtOrder`s and filling them by funding the upfront purchase of the target NFT could look much like [Bloqboard](https://app.bloqboard.com/), or integrate directly with their [API](https://bloqboard.com/api). 

### Contributors
- [Yorke Rhodes IV](https://github.com/yorhodes)
- [Achal Srinivasan](https://github.com/achalvs)
