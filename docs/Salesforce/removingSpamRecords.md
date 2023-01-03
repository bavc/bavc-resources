---
layout: page
title: Removing Spam Records
parent: Salesforce
---


# Removing Spam Records

Whenever someone fills out a Preservation Service Inquiry, the form will automatically create a Salesforce record for the Contact, Organization, and Opportunity. We get a fair amount of spam bots filling out this form, so you'll need to delete these as they come in or they'll make a mess in Salesforce.

Spam records are identified by a bunch of nonsense in the inquiry fields, and often contain a bunch of Cyrillic text. You should be receiving inquiries in your email. As soon as you see a spam inquiry come in you should go to Salesforce and delete the associated records that were created.

The easiest way to delete the records is to search for the organization name and delete the organization. Deleting an organization in Salesforce should delete the related Opportunities and Contacts. However, it's good to double check that the related records were also deleted. If all else fails, just make sure to delete the Contact, Organization, and Opportunity records associated with the spam inquiry. 
