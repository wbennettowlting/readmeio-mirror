---
title: Harbor Basics
excerpt: This section provides a brief, high-level walkthrough of the Harbor ecosystem and introduces key concepts you’ll encounter throughout the integration, including applications, customers, and transfers.
x-content:
  excerpt: This guide provides an introduction to getting started with the Harbor API, including how to authenticate requests, create customers, link blockchain addresses or bank accounts, and make API calls with the required headers and parameters.
hidden: false
metadata:
  robots: index
x-privacy:
  view: public
---
### Application

Once you have access to the Harbor Portal and API, you will be assigned an **application** associated with your organization. Your application serves as the primary container for your Harbor activity, including the customers you create and the transfers you initiate.

The application makes the following distinctions:

- **Harbor**: Refers to us, the platform and service provider.
- **Application**: Refers to you, the integration partner using the Harbor API.
- **Customer**: Refers to the individuals or businesses managed by an Application.


### Customers

Within your application, you can create and manage **Customers** through the Harbor Portal or API. Customers can be either individuals or businesses, and you can execute transfers on their behalf through the Portal or API.

Before you can execute transfers on behalf of a Customer, the Customer must complete the required verification process: **KYC** for individuals and **KYB** for businesses.

Due to compliance requirements Harbor manages this verification process, which we refer to as **Onboarding**.

<Accordion title="Can I be a Customer of my own application?" icon="fa-solid fa-message-question">
Yes. If your organization also wants to process transfers through Harbor, you can onboard your organization as a **Business Customer** within your own application and initiate the **KYB** onboarding process.

</Accordion>


### Onboarding

Before transfers can be initiated on behalf of a Customer, the Customer must complete the required **Onboarding** process.


<Callout icon="📘" theme="info">
  #### Sandbox vs. Production

  - In **Sandbox**, onboarding is approved automatically, usually within 1–2 minutes. 
  - In **Production**, Harbor reviews each submission, which can take 1–2 business days.
</Callout>

Onboarding consists of two main steps:


**Step 1: Accept the agreement**

The Customer receives a link to review and accept the required service agreement.

**Step 2: Provide information**

After accepting the agreement, the Customer provides the information required for verification. For individuals, this includes personal and identity information. For businesses, this includes business information and details about associated individuals.

Harbor uses this information to complete the required KYC or KYB verification.

<Accordion title="Can I submit customer information through the API?" icon="fa-solid fa-message-question">
Yes. Customer information can be submitted either through the **hosted link page** using the provided forms or programmatically through the **Harbor API** using the available endpoints.
</Accordion>


### Transfers

**Transfers** can be initiated on behalf of a customer, whether an individual or a business, once their onboarding is complete. A transfer can be an on-ramp, an off-ramp, or an on-chain transfer.

The transfer process has three main steps.

**Step 1: Create a quote**

Create a quote by gathering the source, the destination, and the commission for the transfer.

**Step 2: Obtain the transfer requirements**

Once the quote is created, obtain the requirements needed to complete the transfer.

**Step 3: Execute the transfer**

Execute the transfer using the quote and the transfer requirements from Step 2.
