# IBM Cloud Framework for Financial Services

## Introduction

This is the documentation repository for the Reference Architecture / Deployment and Configuration Guide for the IBM Cloud Framework for Financial Services.

## Cloud Docs locations

* [Production](https://cloud.ibm.com/docs/framework-financial-services)
* [Staging](https://test.cloud.ibm.com/docs/framework-financial-services)

## Content Design and Development in IBM Cloud

To learn about using the Cloud Docs framework, see [Content Design and Development in IBM Cloud](https://test.cloud.ibm.com/docs/writing?topic=writing-get-started-onboarding).

## Richie-enabled

This repo is enabled with the [markdown enricher (Richie)](https://github.ibm.com/cloud-docs-automation/md-enricher-for-cicd/wiki/Overview) which allows writers to single-source content delivery from one branch to multiple branches in the same or different GitHub repositories. It also has a number of other really [cool features](https://github.ibm.com/cloud-docs-automation/md-enricher-for-cicd/wiki/Usage#Versioning-content).

All PR's for new content should be made against the `source` branch. When content is committed to the `source` branch, the Travis script invokes the Richie. When Richie finishes its processing, it pushes the result to the `draft` branch.

## Change management process

Anyone making updates to content in this repo should follow the [change management process](https://github.ibm.com/cloud-docs-allowlist/framework-financial-services/blob/draft/internal/change-management.md).

## Downloadable static content (e.g., spreadsheets)

These docs depend on having a link to the downloadable spreadsheet of controls. The spreadsheet is stored in the [Cloud Docs downloads repo](https://github.ibm.com/cloud-content/downloads/tree/publish/framework-financial-services). See [Hosting downloadable content assets](https://test.cloud.ibm.com/docs/writing?topic=writing-download-assets) for details on the mechanics of adding, updating, and linking to downloadable content.

## Internal FAQs

An _internal_ [Technical FAQs for IBM Cloud for Financial Services](https://test.cloud.ibm.com/docs/framework-financial-services?topic=framework-financial-services-faqs-internal) is also provided.

## Checking build status

Build status can be found in the [`#docs-framework-financial-services`](https://ibm-cloudplatform.slack.com/archives/C01EKERFT7B) Slack channel.

## Looking for broken links

See [broken-links.csv](https://github.ibm.com/cloud-docs-allowlist/framework-financial-services/blob/draft-cqd/broken-links/broken-links.csv) for Content Quality Dashboard results showing broken links in this repository.


