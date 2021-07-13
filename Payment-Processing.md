# Payment Processing

Anything related to processing payments in Salesforce

---

## Definitions

- `Purchase Order (PO)`: A commercial document and first official offer issued by a buyer to a seller indicating types, quantities, and agreed prices for products or services. It is used to control the purchasing of products and services from external suppliers. [[1]](https://en.wikipedia.org/wiki/Purchase_order)
- `Automated Clearing House Transactions (ACH)`: ACH transfers are electronic, bank-to-bank money transfers processed through the [Automated Clearing House (ACH) Network](https://www.investopedia.com/terms/a/ach.asp). [[1]](https://www.investopedia.com/ach-transfers-what-are-they-and-how-do-they-work-4590120)
  - Getting your pay through direct deposit or paying your bills online through your bank accounts are just two examples of ACH transfers. You can also use ACH transfers to make single or recurring deposits into an individual retirement account (IRA), a taxable brokerage account, or a college savings account.2 Business owners can also use ACH to pay vendors or receive payments from clients and customers.
- `Payment Card Industry (PCI)`

---

## Payment Gateway

- <https://www.paypal.com/us/brc/article/what-is-a-payment-gateway>
- [Processing Payments with Payment Gateways](https://help.salesforce.com/articleView?id=sf.blng_payment_gateways.htm&type=5)
  - > Salesforce Billing supports payment interfaces to process credit card and ACH transactions. Payment gateways are external service providers that process these electronic payments.
- [Get Started With Payment Gateways](https://help.salesforce.com/articleView?id=sf.blng_payment_gateway_overview.htm&type=5)
  - > Payment gateways are external platforms that act as a bridge for communications between Salesforce Billing and customer banks during the payment transaction process. Your developers configured the API to help Salesforce Billing send customer payment information to the gateway.
- Sits between Salesforce and the bank (payment processor)
  - By using a payment gateway you could utilize multiple banks or switch banks without changing your integration or backend processes.
- [Payment Gateway Adapters - Apex Developer Guide](https://developer.salesforce.com/docs/atlas.en-us.apexcode.meta/apexcode/apex_commercepayments_adapter_intro.htm)

---
