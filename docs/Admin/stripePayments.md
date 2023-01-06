---
layout: page
title: Stripe Payments
parent: Admin
---

# Using Stripe To Process Client Payments

We use the website [Stripe](dashboard.stripe.com/) to process client payments for smaller projects. Larger projects should be processed with checks or wire transfer becuase Stripe charges us a fee.

## Creating a Quote or invoice

This is covered in the article titled [Creating Quotes and Invoices]({{ site.baseurl}}/docs/Salesforce/quotesInvoices.html) in the Salesforce section of our documentation

## Logging into stripe

Stripe login information can be found in our password manager. Ask finance or ops for help if you can't log in

## Creating an invoice in Stripe

Log into stripe and follow these directions

* Press the _Create_ button in the top right corner of the strip screen
* Press _Invoice_ from the dropdown menu
* Add in the clients name in the _Find or add customer_ window. If the client already exists you can select them from the dropdown
* Add the client's email
* In the _Items_ section, add in something descriptive, such as _Prservation Services_ and the _Salesforce Quote Number_. Including the quote number is very helpful for the finance department
* Once the numbers all match up, press Review Invoice
* In the pop up window, add _preservation@bavc.org_ to the CC so that we get a record of the Invoice
* Select _Send Invoice_

That's it, you're done!


## Tracking Payments

If you want to see if someone has paid you can search for either the salesforce quote number, the Stripe invoice number, or the client's name in the Stripe search field. 
