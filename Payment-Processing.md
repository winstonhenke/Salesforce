# Payment Processing

Anything related to processing payments in Salesforce

---

## General

Payment Gateway

- <https://www.paypal.com/us/brc/article/what-is-a-payment-gateway>
- [SF: Processing Payments with Payment Gateways](https://help.salesforce.com/articleView?id=sf.blng_payment_gateways.htm&type=5)
  - > Salesforce Billing supports payment interfaces to process credit card and ACH transactions. Payment gateways are external service providers that process these electronic payments.
- [SF: Get Started With Payment Gateways](https://help.salesforce.com/articleView?id=sf.blng_payment_gateway_overview.htm&type=5)
  - > Payment gateways are external platforms that act as a bridge for communications between Salesforce Billing and customer banks during the payment transaction process. Your developers configured the API to help Salesforce Billing send customer payment information to the gateway.
- Sits between Salesforce and the bank (payment processor)
  - By using a payment gateway you could utilize multiple banks or switch banks without changing your integration or backend processes.
