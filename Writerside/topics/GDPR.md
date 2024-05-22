# GDPR Research

This is a document which looks at the GDPR, what it is and how my application needs to comply with it.

## What is GDPR?

According to this article published by the Netherlands Enterprise Agency, RVO and the Netherlands Chamber of Commerce, KVK [[1]](#references) the GDPR is:

> The GDPR is a European privacy regulation. It ensures the careful processing of personal data by businesses and organisations. For instance, you must have a good reason to process personal data. And you are not allowed to gather and use more data than is necessary. These rules apply across the European Economic Area (EEA). GDPR stands for General Data Protection Regulation. In the Netherlands, the GDPR is often referred to as the AVG or Algemene Verordening Gegevensbescherming.

So in short, the GDPR is a regulation that ensures that businesses and organisations are careful with the processing of personal data. not only must they be careful with the data, they must also have a good reason as to they are collecting and processing the data.

These rules apply to all businesses and organisations that are based in the European Economic Area (EEA). It contains a number of European countires including the Netherlands, Belgium, Germany, France, Italy, Spain, Portugal, Sweden, Finland, Norway, Iceland, and many more [[2]](#references).

## What is "processing personal data"

The article [[1]](#references) also explains what is meant by "processing personal data":

> 'Processing personal data’ includes:
> 
> - collecting
> 
> - storing
> 
> - using
> 
> - forwarding
> 
> - sharing
> 
> - distributing
> 
> - merging
>
> For example, if you sell products to customers online, you collect their personal details to be able to ship their orders. Or if you send a newsletter to a distribution list, you are using personal data.

Meaning any action that involves the handling of personal data is considered processing personal data.

The article continues by giving examples of personal data:

> Names and addresses, telephone numbers, postcodes, and house numbers are all personal data. Sensitive data, such as someone’s race, sexual orientation, religion, or health, are called 'special personal data'. It is not allowed to process special or criminal personal data, unless an exception has been made for you in the law.

This part mentions standard personal data and special personal data. Standard personal data is data that can be used to identify a person, such as names, addresses, and telephone numbers. 

Special personal data is data that is more sensitive, the difference is that special personal data can not be processed _at all_ unless an exception has been made for you in the law.

## How does my application need to comply with the GDPR?

The article [[1]](#references) also gives a list of 10 steps that businesses and organisations can take to comply with the GDPR. I will go through each of these steps and see whether they are relevant for my project and how I can implement them.

### 1. Get informed about the GDPR and check if you are allowed to process personal data

First it talks about getting informed about the GDPR, it links to a page on the European Commission website [[3]](#references) which contains information about the GDPR. This is a handy page containing information about the GDPR and how it affects businesses and organisations.

### 2. Check whether you may process personal data

This step is about checking whether you are even allowed to process personal data. They mention 6 circumstances under which you are allowed to process personal data:

> You are allowed to process personal data in these 6 circumstances:
>
> - You have permission from the person involved;
>
> - You need the data to fulfil an agreement. For instance, you need address details to deliver your product to your customer;
>
> - You need the data to meet a legal obligation;
>
> - You need the data to protect someone’s life or health, and you cannot ask that person for permission;
>
> - You need the data to execute a task in the general interest;
>
> - You have a justified cause for processing the data. For instance, you must process personal data in your personnel records to be able to pay wages.

My project includes the creating of an account in order to access certain features, when creating this account, the user will have to provide an email address and a password. This is necessary to fulfil an agreement, as the user needs to create an account in order to access the features of the application.

### 3. Inform your customers of their rights under the GDPR

This step is about informing your customers of their rights under the GDPR. This includes the right to access their data, the right to edit their data, the right to delete their data, and the right to object to the processing of their data. The article mentions these:

> - View, edit, and delete their data;
>
> - Curb or withdraw any permissions previously given by them;
>
> - Request their data to facilitate their move to a different company service provider. This is called data portability.

With the account-system, this can be achieved by giving the user the ability to view and edit their data, and to delete their account. This can be done by providing a settings page where the user can view and edit their data, and a delete account button which will delete the account and all associated data with this account.

### 4. Keep a record of your processing activities

The articles explains this as the following:

> You have to prove you are accountable for how you process data. To do so, you are obliged to keep a record of how and why you process personal data. This is called a processing register. The record has to contain information on where the data comes from and who you share it with. You must be able to notify the organisations you share data with of any changes or deletions of customer data.

When following the link [[4]](#references), it navigates to a page with the titel "When do you need a processing register". This page explains when you need a processing register:

> Do you employ more than 250 staff? You have to keep a register.
> 
> Do you employ fewer than 250 staff? You do not need to keep a register.

It continues by saying that in certain situations you need to have a register, even if you employ fewer than 250 people:

> - You regularly process personal data (this will be the case for most organisations).
> 
> - You process data that can pose risks for the rights and freedoms of the persons involved.
> 
> - You process special personal data (data around health, religion, political preferences, or criminal records).

In order to comply with this, the page mentions the creation of a privacy declaration, which contains information about the processing of personal data. This can be done by creating a privacy policy page, which contains information about the processing of personal data. If relevant, the page also gives a list of information that should be included in the privacy declaration.

### 5. Find out if you need to perform a Data Protection Impact Assessment (DPIA)

This section mentions that you only need to perform a DPIA if the application processes data with a high privacy risk. The section continues by explaining when you run a high privacy risk:

> - Evaluate personal aspects in a systematic and extensive manner, based on automatic processing, including profiling, and if on these evaluations you base decisions that have consequences for people;
> 
> - Process special personal data on a large scale, or process criminal data;
> 
> - Systematically follow people on a large scale in a public access area, for instance by using CCTV.

Since my application does not process special personal data, and does not follow people on a large scale in a public access area, a DPIA is not necessary.

### 6. Take privacy into account when designing new products or services

This section is about taking privacy into account when designing new products or services. It explains it as the following:

> When you devise new products or services, ensure that personal data are already well-protected in the design phase. This is referred to as ‘privacy by design’. You should not process more personal data than is necessary.

It then gives a list of examples:

> - An app should not record the user’s location without good cause;
> 
> - Do not pre-check the ‘yes, I want to receive offers’ radio button on your website;
> 
> - Do not ask for more information than necessary to record a subscription to a newsletter.

My application currently saves very little personal data anyway, although this is something to keep in mind when adding new features to the application.

### 7. Find out if you need a data protection officer

This section is about finding out if you need a data protection officer. The article mentions that you need a data protection officer if:

> Does your company process data on a large scale? Then you may be obliged to employ a Data Protection Officer or DPO. This is called a Functionaris Gegevensbescherming or FG in Dutch. A DPO is responsible for checking if your organisation acts in accordance with the GDPR.

Although my application does not process data on a large scale, which is mostly data that isn't even personal data, it is still something to keep in mind when the application grows.

### 8. Document and report data leaks

Here it talks about the need to notify your users when a data leak occurs. It mentions that you have to report a data leak to the DPA:

> You have to report every serious data leak to the DPA. Also, you must record and document every data leak in your organisation, even the internal ones that you do not have to report. View the guidelines to find out which data leaks to report. These guidelines have not been made final. You only have to notify the persons whose data are involved in the data leak, if it has serious consequences for their rights and freedoms.

This just means that you have to keep track of all data leaks, even if it is just an internal one, that does not require reporting. And when a leak does occur, you have to notify the persons whose data are involved in the data leak.

This is also something to keep in mind for the future. The application currently stores the user's email address and password, which is sensitive data, although the other data is just the articles and/or comments that the user has created.

### 9. Draw up a data processor agreement

This section is about drawing up a data processor agreement. It mentions that you have to draw up a data processor agreement if you hire a third party to process personal data for you:

> Do you work with companies that process personal data on your behalf and which follow your instructions? Make sure you draw up a data processor agreement in accordance with GDPR Articles 28 and 29.

In the case of my application, I use an external service called Auth0, which is used for authentication. This means that I have to draw up a data processor agreement with Auth0. Luckily this is easy since Okta, the company that owns Auth0, has a page with a data processing agreement that can be signed [[5]](#references).


### 10. Determine the supervisor for your company

This section is about determining the supervisor for your company. It mentions that you have to determine the supervisor for your company if your organisation is active in several European countries or if your data processing activities affect several EU member states:

> Is your organisation active in several European countries? Or do your data processing activities affect several EU member states? The GDPR requires you to deal with only one privacy supervisor, for instance, the Dutch DPA. This is called the one-stop-shop mechanism.

My application is based in the Netherlands, so the Dutch DPA would be the supervisor for my company. With a platform like mine, where anyone can access it, it's very likely that the data processing activities affect several EU member states.

## Conclusion

I looked at the 10 steps that businesses and organisations need to take to comply with the GDPR, and saw how my application needs to comply with them. I found that a lot of the steps are not relevant for my application, as it does not process personal data on a large scale, and does not process special personal data. 

The only steps that are relevant are the creation of a privacy policy page, and the drawing up of a data processor agreement with Auth0 and smaller steps that are just important for the futures like keeping track of data leaks and taking privacy into account when designing new products or services.

Here is a quick list of the steps and whether they are relevant for my application:

1. Get informed about the GDPR and check if you are allowed to process personal data - Relevant and always ongoing
2. Check whether you may process personal data - Relevant and compliant
3. Inform your customers of their rights under the GDPR - Relevant
4. Keep a record of your processing activities - Relevant
5. Find out if you need to perform a Data Protection Impact Assessment (DPIA) - Not relevant
6. Take privacy into account when designing new products or services - Relevant and always ongoing
7. Find out if you need a data protection officer - Not relevant
8. Document and report data leaks - Relevant and important for the future
9. Draw up a data processor agreement - Relevant and handled by Okta (Auth0)
10. Determine the supervisor for your company - Relevant

## References
- [1] 10 steps to comply with the GDPR in the Netherlands. (z.d.). Business.gov.nl. https://business.gov.nl/running-your-business/business-management/administration/how-to-make-your-business-gdpr-compliant/
- [2] European Economic Area (EEA). Eurostat. https://ec.europa.eu/eurostat/statistics-explained/index.php?title=Glossary:European_Economic_Area_(EEA)
- [3] Rules for business and organisations. (z.d.). European Commission. https://commission.europa.eu/law/law-topic/data-protection/reform/rules-business-and-organisations_en
- [4] When do you need a processing register. (z.d.). https://business.gov.nl/regulation/when-do-you-need-processing-register/
- [5] Trust and Compliance Documentation. (z.d.-b). Okta, Inc. https://www.okta.com/trustandcompliance/