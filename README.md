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
- NFT is purchased programatically by customTermsContract
- NFT is collateralized using the `ERC721Collateralizer`

### References
- The custom terms contract is defined as: `registerTermStart(bytes32 agreementId, address debtor)`
    - purchase NFT, revert on failure
    - collateralize NFT

- `registerRepayment`, `getExpectedRepaymentValue`, `getValueRepaidToDate`, `getTermEnd Timestamp` 
are defined by [ERC721CollateralizedSimpleInterestTermsContract](https://github.com/dharmaprotocol/charta/blob/master/contracts/examples/ERC721CollateralizedSimpleInterestTermsContract.sol)

### Contributors
- [Yorke Rhodes IV](https://github.com/yorhodes)
- [Achal Srinivasan](https://github.com/achalvs)