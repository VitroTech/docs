# Research About Hardware Claiming with AWS Marketplace

> This document contains the fundamentals and the most important findings about
> preparation and deployment process of an application in AWS Marketplace.

### AWS Marketplace Requirements

To be able to sell application through the Marketplace the below requirements
must be met:

 - **An AWS Seller Account**, which can be created [here](https://aws.amazon.com/marketplace/management/tour/).
   - *Whats needed: US bank account, [valid payment method](https://portal.aws.amazon.com/gp/aws/developer/account?ie=UTF8&action=payment-method) and submitted [W9 Form](https://core-docs.s3.amazonaws.com/documents/asset/uploaded_file/142225/W-9.pdf) (AMI option only).*


### Adding product to the Marketplace

Adding application to Marketplace listing is a multi-step process.

- The software itself needs to be:
  - publicly available,
  - production ready (not in beta or early access),
  - have a well defined customer support,
  - have the means to ensure regular updates,
  - follow the marketing guidelines of AWS Marketplace,
- Choose the type of product delivery solution from the ones listed below:
  - **Amazon Machine Image (AMI)** - the seller gives access only to the image of the application and buyer needs to maintain the infrastructure needed (cost o the second is calculated by AWS) - [link to the official guidelines](https://docs.aws.amazon.com/marketplace/latest/userguide/saas-guidelines.html),
  - **Software as a Service (SaaS)** - the seller provides access to the application with necessary infrastructure and the buyer is billed for both (the seller create rules for charges calculation),
- Select the pricing model from the ones listed below:
  - **Annual pricing** - this option is only noted, because it was not in the scope of this research,
  - **Usage pricing** - reported to the *AWS Marketplace Metering Service* by the seller on a hourly basis (reported by the software deployed on the users account; the usage is billed monthly). There are four available usage categories: *users*, *hosts*, *bandwidth* and *data*. They can be used to create up to 24 dimensions e.g. *$0.009 per request* or *20,000 requests per month for $100* that can coexist - it needs to be noted that all pricing model changes needs an AWS Marketplace approval. The process can take from 30 to 90 days,
- **Product Form Submission** - an product form needs to contain the following information about the product:
  - name,
  - description,
  - highlights,
  - release notes,
  - usage instructions,
  - upgrade instructions,
  - product category and keywords,
  - End User License Agreement (EULA)
- Configure product registration website, as described [in this document](https://docs.aws.amazon.com/marketplace/latest/userguide/saas-prepare.html),
- Additionally there's also an [SaaS product integration checklist](https://docs.aws.amazon.com/marketplace/latest/userguide/aws-marketplace-integration-checklist.html) prepared by Amazon,

### Application usage reporting

As was mentioned before, for the SaaS model a seller is responsible for usage report creation and delivery to the *AWS Marketplace Metering Service*. This report needs to be send every hour for successful billing.

The whole process is described in detail on [Seller reports and data feeds](https://docs.aws.amazon.com/marketplace/latest/userguide/reports-and-data-feed.html) section of the AWS Marketplace documentation.

- [Link to the example implementations usage reporting](https://docs.aws.amazon.com/marketplace/latest/userguide/saas-code-examples.html).

### Example applications

- [Fybr Insights](https://aws.amazon.com/marketplace/pp/B074Z86M48?qid=1602078242950&sr=0-5&ref_=srh_res_product_title) - an app to analyze data from sensors and controllers running on Fybr's IoT Platform (priced by every request),
- [Benchy](https://aws.amazon.com/marketplace/pp/B07TZNMDT8?qid=1602078242950&sr=0-6&ref_=srh_res_product_title) - platform for M2M encrypted IoT data exchange (priced by every request),
