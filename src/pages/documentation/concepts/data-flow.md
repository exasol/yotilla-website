---
layout: base
sidebar: true
meta:
  title: Data Flow
eleventyNavigation:
  key: Data Flow
  parent: Concepts
  order: 1
---

# Data Flow

***

<p class="lead">
This is an overview of the data flow when using Yotilla to build a Data Warehouse. For more information about the respective steps in this process, see The Yotilla Workflow.
</p>

***

## General Concepts
The source data for the data warehouse must first be extracted from the source systems and loaded into an external staging area on the target system. For Yotilla, the external staging area on the target system will then be seen as the source system.

Extracting and loading data from the source systems into the external staging area cannot be done by Yotilla. This operation must be carried out by the target system, or using an ELT/ETL tool such as Keboola.

The next step is to load the source data that is present in the external staging area into a Yotilla-managed landing zone on the target system. This operation is done in Yotilla.

Yotilla does not store any data, only metadata. All data will be stored on the target system.

***

## Staging Source Data

<img src="{{ '/assets/images/documentation/yotilla-staging-schema.png' | url }}" alt="Staging on target" class="img-fluid" />

***

## Persistent Staging
The Yotilla-managed landing zone is defined as a persistent staging area (PSA). This means that in case of a failure of the target system that results in loss of data, Yotilla will be able to regenerate the core layer and the data mart layer of the data warehouse, but not the landing zone itself.

<p class="callout callout-warning">
If the external staging area is defined as a PSA, the entire data warehouse can be recreated in case of failure, including the Yotilla-managed landing zone.
</p>

<p class="callout callout-info">
If the external staging area is defined as a persistent staging area (PSA), sources with large volumes of data may cause very high storage consumption on the target system.
</p>

Persistent staging on the external staging area must be set up and managed in the external system, it cannot be configured in Yotilla.

The following requirements apply when using persistent staging on the external staging area:

* Only one source attribute in each source must be marked as external stage load time.
* The same data time value must be inserted into the source column marked as external stage load time in all rows of an extraction.
* Data must never be removed or truncated during the extraction of source data.
* For more details about configuring sources, see Edit Sources.
