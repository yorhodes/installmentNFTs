Detailed Flow:

1. Buyer identifies desirable but unaffordable NFT listed for programmatic sale.
- OpenSea provides an API for [buying items](https://projectopensea.github.io/opensea-js/#buying-items): 
2. Buyer instantiates DebtOrder expressing borrower intent on relayer with following details:
- personal ethereum account
- principal of 0
- address of customTermsContract template
- customTermsContract params (including programmatic purchase details)
- fees to relayer/underwriter
3. Excess liquidity holder can fill DebtOrder and become lender:
- lender funds customTermsContract with NFT price in unit
- NFT is purchased programatically by customTermsContract
- NFT is collateralized

Custom Terms Contract:
registerTermStart(bytes32 agreementId, address debtor)
- purchase NFT, revert on failure
- collateralize NFT

registerRepayment, getExpectedRepaymentValue, getValueRepaidToDate, getTermEndTimestamp 
as defined by [ERC721CollateralizedSimpleInterestTermsContract](https://github.com/dharmaprotocol/charta/blob/master/contracts/examples/ERC721CollateralizedSimpleInterestTermsContract.sol)
