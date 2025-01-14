---

copyright:
  years: 2020, 2024
lastupdated: "2024-11-19"

keywords:

subcollection: framework-financial-services

---

{{site.data.keyword.attribute-definition-list}}

# Security categorization, functionality and assurance
{: #system-security-categorization}

The reference architectures and associated guidance are intended to help you meet the [control requirements](/docs/framework-financial-services-controls) in the {{site.data.keyword.framework-fs_notm}}. However, there is no one-size-fits-all approach for designing solutions for regulated industries, and you have latitude to make adjustments based on your situation. Ultimately, you are responsible for your choices based on careful consideration of organizational policies, data classification requirements, vulnerability and threat information, applicable regulations, compliance standards, and risk appetite. One approach to help you make decisions is to do a security categorization of your workloads and data.
{: shortdesc}



## Framework for security categorization
{: #system-security-categorization-framework}

The [Standards for Security Categorization of Federal Information and Information Systems (FIPS 199)](https://csrc.nist.gov/publications/detail/fips/199/final){: external} provides a framework for security categorization of information and information systems. The security categories it defines are based on the "potential impact on an organization should certain events occur which jeopardize the information and information systems needed by the organization to accomplish its assigned mission, protect its assets, fulfill its legal responsibilities, maintain its day-to-day functions, and protect individuals."

FIPS 199 lists the following three security objectives common to information and information systems as defined by the Federal Information Security Management Act of 2002 (FISMA):

Confidentiality
:   Preserving authorized restrictions on information access and disclosure, including means for protecting personal privacy and proprietary information.

Integrity
:   Guarding against improper information modification or destruction, and includes ensuring information non-repudiation and authenticity.

Availability
:   Ensuring timely and reliable access to and use of information.


<br></br>

With the security objectives in mind, FIPS 199 goes on to define the following three levels of potential impact on organizations or individuals should there be a breach of security (i.e., a loss of confidentiality, integrity, or availability).

High
:   The loss of confidentiality, integrity, or availability could be expected to have a severe or catastrophic adverse effect on organizational operations, organizational assets, or individuals.

Moderate
:   The loss of confidentiality, integrity, or availability could be expected to have a serious adverse effect on organizational operations, organizational assets, or individuals.

Low
:   The loss of confidentiality, integrity, or availability could be expected to have a limited adverse effect on organizational operations, organizational assets, or individuals.

## Using FIPS 199 guidelines
{: #system-security-categorization-usage}

The following steps are used to do a system security categorization:

1. Identify all information types supported by your information system.
1. Record the potential impact for each security objective for each information type.
1. Determine the security categorization of each information type by selecting the highest impact of the associated security objectives.
1. Determine the overall system security categorization by choosing the highest security category.

The remaining sections show some examples of applying this process.

### Example 1: Payment processing system
{: #system-security-categorization-usage-example-1}

The following table shows an example for a fictional payment processing system. Two information types have been identified and are listed in the first column. The next three columns show the assessed impact for the Confidentiality, Integrity, and Availability security objectives for each information type.

| Information type                                                   | Confidentiality | Integrity | Availability |
|--------------------------------------------------------------------|-----------------|-----------|--------------|
| Payment information (account/card numbers, account balance, and so on) | High            | High      | High         |
| Sensitive information (PII, PHI, and so on)                             | High            | High      | High         |
{: caption="Table X. Example security categorization for information types for a payment processing system" caption-side="bottom"}

The following table refines the results from the previous table. It shows the highest impact value in each of the columns for the Confidentiality, Integrity, and Availability security objectives from the previous table. The last column shows the highest impact across the three security objective columns. In this case, the overall security categorization for the payment processing system is High.

|                           | Confidentiality | Integrity | Availability | Overall result |
|---------------------------|-----------------|-----------|--------------|----------------|
| System categorization     | High            | High      | High         | **High**       |
{: caption="Table X. Example overall system security categorization for payment processing system" caption-side="bottom"}

### Example 2: Training system
{: #system-security-categorization-usage-example-2}

The following table shows an example for a fictional training system. Two information types have been identified and are listed in the first column. The next three columns show the assessed impact for the Confidentiality, Integrity, and Availability security objectives for each information type.

| Information type                                                          | Confidentiality | Integrity | Availability |
|---------------------------------------------------------------------------|-----------------|-----------|--------------|
| Training information (training course data, completion status, and so on) | Low            | Moderate      | Moderate         |
| Public information (course name, title, and so on)                        | Low            | Low      | Low         |
{: caption="Table X. Example security categorization for information types for a training system" caption-side="bottom"}

The following table refines the results from the previous table. It shows the highest impact value in each of the columns for the Confidentiality, Integrity, and Availability security objectives from the previous table. The last column shows the highest impact across the three security objective columns. In this case, the overall security categorization for the training system is Moderate.

|                           | Confidentiality | Integrity | Availability | Overall result |
|---------------------------|-----------------|-----------|--------------|----------------|
| System categorization     | Low            | Moderate      | Moderate         | **Moderate**       |
{: caption="Table X. Example overall system security categorization for a training system" caption-side="bottom"}

### Example 3: Back-end of mobile application
{: #system-security-categorization-usage-example-3}

The following table shows an example for a back-end of a mobile application. Two information types have been identified and are listed in the first column. The next three columns show the assessed impact for the Confidentiality, Integrity, and Availability security objectives for each information type.

| Information type                                                          | Confidentiality | Integrity | Availability |
|---------------------------------------------------------------------------|-----------------|-----------|--------------|
| Transactional information (User request including PII, preferences) | Moderate         | Moderate         | Moderate         |
| Routine administrative information (non-privacy related information)    | Low            | Low      | Low         |
{: caption="Table X. Example security categorization for information types for a back-end of a mobile application" caption-side="bottom"}

The following table refines the results from the previous table. It shows the highest impact value in each of the columns for the Confidentiality, Integrity, and Availability security objectives from the previous table. The last column shows the highest impact across the three security objective columns. In this case, the overall security categorization for the fraud management system is High.

|                           | Confidentiality | Integrity | Availability | Overall result |
|---------------------------|-----------------|-----------|--------------|----------------|
| System categorization     | Moderate       | Moderate    | Moderate      | **Moderate**       |
{: caption="Table X. Example overall system security categorization for a back-end of a mobile application" caption-side="bottom"}

### Example 4: Fraud management system
{: #system-security-categorization-usage-example-4}

The following table shows an example for a fictional fraud management system. Two information types have been identified and are listed in the first column. The next three columns show the assessed impact for the Confidentiality, Integrity, and Availability security objectives for each information type.

| Information type                                                          | Confidentiality | Integrity | Availability |
|---------------------------------------------------------------------------|-----------------|-----------|--------------|
| Transactional information (payments, purchases, charge backs, and so on) | High            | High      | Moderate         |
| Routine administrative information (non-privacy related information)    | Low            | Low      | Low         |
{: caption="Table X. Example security categorization for information types for a fraud management system" caption-side="bottom"}

The following table refines the results from the previous table. It shows the highest impact value in each of the columns for the Confidentiality, Integrity, and Availability security objectives from the previous table. The last column shows the highest impact across the three security objective columns. In this case, the overall security categorization for the fraud management system is High.

|                           | Confidentiality | Integrity | Availability | Overall result |
|---------------------------|-----------------|-----------|--------------|----------------|
| System categorization     | High            | High      | Moderate         | **High**       |
{: caption="Table X. Example overall system security categorization for a fraud management system" caption-side="bottom"}

### Example 5: Network security system
{: #system-security-categorization-usage-example-5}

The following table shows an example for a fictional network security system. Two information types have been identified and are listed in the first column. The next three columns show the assessed impact for the Confidentiality, Integrity, and Availability security objectives for each information type.

| Information type                                                          | Confidentiality | Integrity | Availability |
|---------------------------------------------------------------------------|-----------------|-----------|--------------|
| Security information (authentication, identification, network monitoring, and so on) | High            | High      | Moderate         |
| Routine administrative information (non-privacy related information)    | Low            | Low      | Low         |
{: caption="Table X. Example security categorization for information types for a network security system" caption-side="bottom"}

The following table refines the results from the previous table. It shows the highest impact value in each of the columns for the Confidentiality, Integrity, and Availability security objectives from the previous table. The last column shows the highest impact across the three security objective columns. In this case, the overall security categorization for the network security system is High.

|                           | Confidentiality | Integrity | Availability | Overall result |
|---------------------------|-----------------|-----------|--------------|----------------|
| System categorization     | High            | High      | Moderate         | **High**       |
{: caption="Table X. Example overall system security categorization for a network security system" caption-side="bottom"}

## Functionality and assurance
{: #system-security-categorization-functionality-and-assurance}

Traditionally, security has been divided into two perspectives: functionality and assurance. Functionality is the security capabilities described by a policy, control framework, or security target.

Assurance is a set of activities that give confidence in the implementation and operation of control requirements. The type of assurance can vary depending on the scope of the control framework and the amount of effort put into confirming the implementation and operation of the control requirements. In this case, the [Financial Services Validation process](/docs/framework-financial-services?topic=framework-financial-services-fs-validated-partner-process) performs assurance activities to enable confidence in the implementation of the controls.

The overall security categorization of the data will guide the functionality and assurance required from a cloud service being used to protect the data. For example, while {{site.data.keyword.hscrypto}} and {{site.data.keyword.keymanagementserviceshort}} have received the same level of assurance, both being Financial Services Validated, they each provide different security functionality. They may be used to protect data with a different security categorization. For a further discussion on the differences, read [Data encryption at rest in IBM Cloud](/docs/framework-financial-services?topic=framework-financial-services-shared-encryption-at-rest#shared-encryption-at-rest-ibm-cloud) for a comparison of functionality.

When it comes to differences in assurance, there are multiple options for audit logging. You can  use {{site.data.keyword.atracker_short}}, that's Financial Services Validated, to forward logs to either {{site.data.keyword.cos_full_notm}} or {{site.data.keyword.messagehub}} (Enterprise Plan) and then use a custom solution for an audit event monitoring application. An alternative is to use Activity Tracker hosted event search, that's not Financial Services Validated, but has audit event monitoring built-in. The overall security categorization of the data should guide whether you need the assurance provided with a Financial Services Validated solution.

When you think about modifying a security solution because of a lower security categorization, think about the two dimensions of functionality and assurance.

## Next steps
{: #next-steps}

If you plan to provide SaaS:

* Review [Best practices for software as a service](/docs/framework-financial-services?topic=framework-financial-services-best-practices), which represent a generic set of implementation principles that apply to SaaS applications on both reference architectures.
* Explore the [reference architectures](/docs/framework-financial-services?topic=framework-financial-services-reference-architecture-overview) and deployment guidance

If you plan to provide software that another organization will operate, review [Best practices for software](/docs/framework-financial-services?topic=framework-financial-services-best-practices-software).
