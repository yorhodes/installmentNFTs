Detailed Flow:

1. Buyer identifies desirable but unaffordable NFT listed for programmatic sale.
    A. OpenSea provides an API for buying items: https://projectopensea.github.io/opensea-js/#buying-items
2. Buyer instantiates DebtOrder expressing borrower intent on relayer with following details:
    A. personal ethereum account
    B. principal of 0
    C. address of customTermsContract template
    D. customTermsContract params (including programmatic purchase details)
    E. fees to relayer/underwriter
3. Excess liquidity holder can fill DebtOrder and become lender:
    A. lender funds customTermsContract with NFT price in unit
    B. NFT is purchased programatically by customTermsContract
    C. NFT is collateralized

Custom Terms Contract:
registerTermStart(bytes32 agreementId, address debtor)
- purchase NFT, revert on failure
- collateralize NFT

registerRepayment, getExpectedRepaymentValue, getValueRepaidToDate, getTermEndTimestamp 
as defined by ERC721CollateralizedSimpleInterestTermsContract
