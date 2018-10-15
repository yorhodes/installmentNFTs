## NFT Installments on [Dharma](https://dharma.io)

### Detailed Flow:

1. Buyer identifies a desirable NFT listed for sale.
- Can integrate with OpenSea API for [buying cryptocollectibles](https://projectopensea.github.io/opensea-js/#buying-items).

2. Buyer instantiates `DebtOrder` expressing intent to borrow on relayer with following details:
- personal wallet address (source of repayment)
- `principal = 0`
- address of `customTermsContract` template
- `customTermsContract` params (including programmatic purchase details)
- fees to relayer (no underwriter, since the NFT is fully collateralized)

3. A party with access to liquidity can fill the `DebtOrder` and become the creditor for the transaction:
- creditor funds `customTermsContract` with NFT price in terms of a listed ERC20 token
- NFT is purchased programatically by `customTermsContract`
- NFT is collateralized using the `ERC721Collateralizer`

### References
- The custom terms contract is defined with `registerTermStart(bytes32 agreementId, address debtor)`
    - purchases NFT, reverts on failure
    - collateralizes NFT

- The necessary methods `registerRepayment`, `getExpectedRepaymentValue`, `getValueRepaidToDate`, and `getTermEndTimestamp` 
have been well defined for our use case in [ERC721CollateralizedSimpleInterestTermsContract](https://github.com/dharmaprotocol/charta/blob/master/contracts/examples/ERC721CollateralizedSimpleInterestTermsContract.sol)

### Interfaces:

The borrower interface for browsing NFTs and instantiating intent to borrow could very easily be implemented as a wrapper around the [OpenSea marketplace](https://opensea.io/assets). Initially, though, we will build a very simple interface for instantiating `DebtOrder`s using our `customTermsContract` and inputting relevant parameters, to be listed on our relayer/lendor interface.

The lendor interface for browsing `DebtOrder`s and filling them by funding the upfront purchase of the target NFT could look much like [Bloqboard](https://app.bloqboard.com/), or integrate directly with their [API](https://bloqboard.com/api). 

### Contributors
- [Yorke Rhodes IV](https://github.com/yorhodes)
- [Achal Srinivasan](https://github.com/achalvs)
