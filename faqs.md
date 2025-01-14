---

copyright:
  years: 2021, 2025
lastupdated: "2025-01-07"

keywords: 

subcollection: framework-financial-services

---

{{site.data.keyword.attribute-definition-list}}



# FAQs for {{site.data.keyword.cloud_notm}} for Financial Services
{: #faqs-framework}


Answers to common questions about {{site.data.keyword.cloud_notm}} for Financial Services and the {{site.data.keyword.framework-fs_full}}.
{: shortdesc}

## What are the supported reference architectures for the {{site.data.keyword.cloud_notm}} for Financial Services?
{: #reference-architectures}
{: faq}

See [references architectures](/docs/framework-financial-services?topic=framework-financial-services-reference-architecture-overview).


## What is a _consumer_ and what is an _application provider_?
{: #consumers-providers}
{: faq}


We refer to the entity that uses (consumes) the application as the _consumer_ and the entity that deploys (provides) the application as the _application provider_. This documentation generally assumes that you are a an application provider.

The application provider and consumer might be the same when the organization providing the application also consumes it (for example, a financial institution deploys an application for internal use by others in their company). Or, they can be different when a third-party ISV provides an application for a financial institution to use.

Application providers who are within financial institutions are not beholden to the {{site.data.keyword.framework-fs_notm}} control requirements.

## What does it mean for a service to be {{site.data.keyword.cloud_notm}} for Financial Services Validated?
{: #financial-services-validated}
{: faq}


{{site.data.keyword.cloud_notm}} for Financial Services Validated designates that an {{site.data.keyword.cloud_notm}} service or ecosystem partner service has evidenced compliance to the controls of the {{site.data.keyword.framework-fs_notm}} and can be used to build solutions that might themselves be validated.

## Should I only use {{site.data.keyword.IBM_notm}} or third-party managed services that are {{site.data.keyword.cloud_notm}} for Financial Services Validated?
{: #not-financial-services-validated}
{: faq}

{{site.data.content.fs-validated-disclaimer}}

## What if I don't want to use managed services, but I want to install and manage my own software?
{: #self-installed-software}
{: faq}

Self-installed (or "bring your own") software from third parties is permitted without special approval. Examples include databases, logging stacks, web application firewalls, and so on. But just like first-party software you might develop and deploy, self-installed third-party software must comply with all of the requirements of the {{site.data.keyword.framework-fs_notm}}. And, it is your responsibility to provide appropriate supporting evidence.

For more information, see [Ensure all usage of software meets {{site.data.keyword.framework-fs_notm}} controls](/docs/framework-financial-services?topic=framework-financial-services-best-practices#best-practices-self-installed-software).

<!-- Just some notes on how to format FAQ entries.

## How should I set up my page?
{: #faq-page-setup}
{: faq}

* Use "FAQs for xxx" as your title, where xxx is the short name with no trademarks.
* Name the file `faqs.md` for URL readability. 
* If you require multiple FAQ files, group under a "FAQs" topic group and use a unique name for each file. 
* Add each question as an H2.
* Use paragraph text following the associated H2 question for each answer. 
* Set the `faq` content type attribute definition at the top of your file.
* Set the `faq` content type attribute on a new line following each H2 question.
* Do not repeat task steps. Summarize and link off to task topic.

## What should I include in my FAQs?
{: #faq-content-include}
{: faq}

Each answer should be approximately one to five sentences. You want to make sure you are not re-documenting information that is already available in documentation because then you'd have to maintain it in two places. If a more detailed explanation for the question exists out in a documentation page, give a concise answer here, and then link out to the doc.

For detailed guidance on what to include on this page, see [FAQs guidance](/docs/developing/writing/faq.html#faqs). You can also check out some examples here: [{{site.data.keyword.cloud_notm}} IAM FAQs](/docs/developing/Access-Management/iamfaq.html#faqs) and [Account FAQs](/docs/account/account_faq.html#accountfaqs).
-->
