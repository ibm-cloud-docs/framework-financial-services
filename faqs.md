---

copyright:
  years: 2021, 2022
lastupdated: "2022-07-29"

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



