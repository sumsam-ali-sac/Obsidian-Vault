
## 1. The Source: Dynamics 365 CRM (Dataverse)

| What | Description |
|------|-----------|
| **System** | Microsoft Dynamics 365 / Power Apps |
| **Database** | **Dataverse** |
| **Contains** | All your CRM records:  
→ Leads, Opportunities, Cases, Emails, Activities  
→ Custom entities: PRP Deals, Write-offs, Facilities, etc. |

> This is the **source of truth** — where users enter and update data daily.

---

## 2. Step 1: Automatic Copy to Synapse (Staging Area)

| What Happens | Why |
|-------------|-----|
| A **background service** called **Azure Synapse Link** continuously copies **every change** (new, updated, deleted) from Dataverse into a **staging database** in **Azure Synapse**. | Dataverse is not built for heavy analytics. Copying data out allows fast reporting without slowing down CRM users. |

### The Staging Database
- **Name**: `dataverse_usc_orgd3f14a56`  
- **Type**: Spark database (fast, scalable, analytics-optimized)  
- **Storage**: Securely stored in Azure Data Lake (behind the scenes)

> Think of this as a **mirror** of the CRM — updated within minutes.

---

## 3. Step 2: From Staging to Clean Warehouse (Azure SQL)

| What Happens | Why |
|-------------|-----|
| Data is **cleaned, transformed, and loaded** from the Synapse staging area into a **relational database** in **Azure SQL**. | Reporting tools (like Power BI) work best with clean, structured tables. |

### The Final Database
- **Name**: Azure SQL Database  
- **Schemas**:  
  - `CRM` → Raw-ish CRM tables (e.g., `activityparty`, `email`)  
  - `dbo` → Modeled tables (e.g., `PRP_Deals`, `DimDealType`)  

> This is the **reporting-ready data warehouse**.

---
## Data Flow Summary (Simple Diagram)

```
Dynamics 365 CRM (Dataverse)
        ↓
[Synapse Link copies changes every few minutes]
        ↓
Synapse Spark Staging (dataverse_usc_orgd3f14a56)
        ↓
[Clean, transform, structure]
        ↓
Azure SQL Database (CRM + dbo schemas)
        ↓
Power BI, Reports, Apps
```

---

## How Data Stays Fresh

| Mechanism           | How It Works       |
| ------------------- | ------------------ |
| **Change Tracking** | Synapse Link adds: |
→ `SinkCreatedOn` (when added)  
→ `SinkModifiedOn` (when updated)


| **Incremental Updates** | Only **recently changed** records are processed daily |
| **Deletes** | Soft-deletes are tracked and removed |

> No full reloads — only what changed.

| TableName                                    | Rows     |
| -------------------------------------------- | -------- |
| account                                      | 62864    |
| account_partitioned                          | 62864    |
| activityparty                                | 15893822 |
| activityparty_partitioned                    | 15893790 |
| annotation                                   | 657748   |
| annotation_partitioned                       | 657725   |
| appointment                                  | 18569    |
| appointment_partitioned                      | 18569    |
| campaign                                     | 632      |
| campaign_partitioned                         | 632      |
| connection                                   | 2012568  |
| connection_partitioned                       | 2012568  |
| connectionrole                               | 79       |
| connectionrole_partitioned                   | 79       |
| contact                                      | 235766   |
| contact_partitioned                          | 235766   |
| drb_addresses                                | 221838   |
| drb_addresses_partitioned                    | 221838   |
| drb_case                                     | 82272    |
| drb_case_partitioned                         | 82272    |
| drb_caseevaluation_partitioned               | 1        |
| drb_caseevaluation                           | 1        |
| drb_caseparty                                | 182918   |
| drb_caseparty_partitioned                    | 182918   |
| drb_contractdetails                          | 1539904  |
| drb_contractdetails_partitioned              | 1539904  |
| drb_county                                   | 779      |
| drb_county_partitioned                       | 779      |
| drb_courtcounty                              | 1347     |
| drb_courtcounty_partitioned                  | 1347     |
| drb_funding                                  | 222732   |
| drb_funding_partitioned                      | 222732   |
| drb_leadsource                               | 56       |
| drb_leadsource_partitioned                   | 56       |
| drb_moneynetworkcard                         | 1647     |
| drb_moneynetworkcard_partitioned             | 1647     |
| drb_partyinsurancecompany                    | 69406    |
| drb_partyinsurancecompany_partitioned        | 69406    |
| drb_payment                                  | 72551    |
| drb_payment_partitioned                      | 72551    |
| drb_portfolio                                | 69       |
| drb_portfolio_partitioned                    | 69       |
| drb_pricing                                  | 308      |
| drb_pricing_partitioned                      | 308      |
| drb_pricingperiod                            | 136010   |
| drb_pricingperiod_partitioned                | 136010   |
| drb_representation                           | 85859    |
| drb_representation_partitioned               | 85859    |
| drb_servicingevaluations                     | 629842   |
| drb_servicingevaluations_partitioned         | 629822   |
| drb_state                                    | 67       |
| drb_state_partitioned                        | 67       |
| drb_subcase                                  | 171      |
| drb_subcase_partitioned                      | 171      |
| email                                        | 626333   |
| email_partitioned                            | 626328   |
| encodian_prpbatch                            | 104      |
| encodian_prpbatch_partitioned                | 104      |
| encodian_prpdeal                             | 7796     |
| encodian_prpdeal_partitioned                 | 7796     |
| encodian_prptransaction                      | 3207     |
| encodian_prptransaction_partitioned          | 3207     |
| EventNotificationErrorsQueue                 | 0        |
| GlobalOptionsetMetadata                      | 2122     |
| lead                                         | 212682   |
| lead_partitioned                             | 212680   |
| list                                         | 210      |
| list_partitioned                             | 210      |
| listmember                                   | 60161    |
| listmember_partitioned                       | 60161    |
| opportunity_partitioned                      | 139762   |
| opportunity                                  | 139762   |
| OptionsetMetadata                            | 2091     |
| phonecall                                    | 657817   |
| phonecall_partitioned                        | 657819   |
| QueryNotificationErrorsQueue                 | 0        |
| queue                                        | 1016     |
| queue_partitioned                            | 1016     |
| queueitem                                    | 229099   |
| queueitem_partitioned                        | 229096   |
| ServiceBrokerQueue                           | 0        |
| StateMetadata                                | 109      |
| StatusMetadata                               | 264      |
| systemuser                                   | 976      |
| systemuser_partitioned                       | 976      |
| TargetMetadata                               | 1731     |
| task                                         | 45865    |
| task_partitioned                             | 45865    |
| usc_accidentlocation                         | 16102    |
| usc_accidentlocation_partitioned             | 16102    |
| usc_backgroundevaluation_partitioned         | 154725   |
| usc_backgroundevaluation                     | 154725   |
| usc_brokercancellation                       | 406      |
| usc_brokercancellation_partitioned           | 406      |
| usc_businessname                             | 12       |
| usc_businessname_partitioned                 | 12       |
| usc_clearverificationsessions                | 5879     |
| usc_clearverificationsessions_partitioned    | 5878     |
| usc_escalatedservicing                       | 1669     |
| usc_escalatedservicing_partitioned           | 1669     |
| usc_escalatedservicingevaluation             | 8296     |
| usc_escalatedservicingevaluation_partitioned | 7941     |
| usc_lawfirmevaluation                        | 5126     |
| usc_lawfirmevaluation_partitioned            | 5126     |
| usc_operationrecovery                        | 564      |
| usc_operationrecovery_partitioned            | 564      |
| usc_operationrecoveryevaluation              | 3645     |
| usc_operationrecoveryevaluation_partitioned  | 3645     |
| usc_opportunity                              | 7500     |
| usc_opportunity_partitioned                  | 7500     |
| usc_programtype                              | 18       |
| usc_programtype_partitioned                  | 18       |
| usc_scheduledinstallment                     | 2392     |
| usc_scheduledinstallment_partitioned         | 2392     |
| usc_underwriterevaluation                    | 153194   |
| usc_underwriterevaluation_partitioned        | 153194   |
| usc_writeoff                                 | 2749     |
| usc_writeoff_partitioned                     | 2749     |
| Total Rows                                   | 48960138 |

## Data Processed Per day
![[Pasted image 20251024202740.png]]

## Container Insights

![[Pasted image 20251024211631.png]]

![[Pasted image 20251024211644.png]]
## Pipelines 

1. DWLoad - Runs after each CMS pipeline
2. import_activty - Runs Monday to Friday at Hours 11, 7 and 17 and minutes 33
3. import_activty_weekend - Runs Saturday and Sunday at Hours 14 , 16 and 10 and minutes 0
4. import_dimensions - Runs Monday to Friday at Hours 7 and 17 and minutes 0
5. importcms -   Runs Monday to Friday at Hours 11,12,13,14,15,16,17,18,7,9,10 and minutes 45
6. importcms_weekend - Runs Saturday and Sunday at Hours 14 , 16 and 10 and minutes 0
7. PRP - Runs Monday to Friday at Hours 6, 12, 15, 19, 11, 13, 14, 16 and minutes 2
8. RunTrellis
9. Test Run


**Stats from last 1000 runs**

|Pipeline name         |TotalRuns|SuccessfulRuns|FailedRuns|SuccessRate|AvgDurationMinutes|MaxDurationMinutes|MinDurationMinutes|
|----------------------|---------|--------------|----------|-----------|------------------|------------------|------------------|
|DWLoad                |266      |265           |1         |99.62      |17.09             |30.08             |11.88             |
|PRP                   |176      |176           |0         |100.0      |5.64              |7.68              |4.42              |
|import_activty        |66       |64            |2         |96.97      |4.54              |12.15             |0.88              |
|import_activty_weekend|24       |24            |0         |100.0      |13.27             |14.93             |9.52              |
|import_dimensions     |44       |44            |0         |100.0      |11.74             |15.93             |6.93              |
|importcms             |242      |242           |0         |100.0      |17.9              |31.83             |11.5              |
|importcms_weekend     |24       |24            |0         |100.0      |35.07             |40.52             |31.02             |

**Deduction**:  RunTrellis and Test Run pipelines are not part of last 1000 runs
### 1. import_dimensions

The "**import_dimensions**" pipeline, using the "df_dimensions" data flow, is an Azure Data Factory ETL process that extracts picklist metadata (options/labels), status codes, and entity records from Dataverse tables (e.g., GlobalOptionsetMetadata, account). It filters by specific option set names/entities, transforms data (e.g., splitting labels, renaming columns), prepares upserts/deletes via alter rows, and loads into SQL Server dimension tables (e.g., DimDealType, DimFacility) to create a star schema for analytics, converting CRM codes to readable values.

| Source Entity           | Sink Table                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | Purpose/Link                                                                                                             |
| ----------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------ |
| GlobalOptionsetMetadata | DimDealType, DimCompromiseStatus, DimClaimType, DimCarrierPriority, DimFundingTypes, DimProgramType, DimCaseStatus, DimRanking, DimLeadStatus, DimReasonforDenial, DimSecondaryReasonDenial, DimPaymentType, DimPayorType, DimCollectionStatus, DimCompromiseReason, DimActivityType, DimAttorneyFee, DimOpportunitiesActivityType, DimBackgroundPriority, DimReasonforDelay, DimContractFullyExecuted, DimAttorneyAcknowledgement, DimDealStatus, DimCollectionFlow, DimCarrierRating, DimHowDidYouHearAboutUs, DimRate, DimRateType, DimFrequency, DimFilingFee, DimRepresentationType, DimBusinessSubordination, DimLocationUnavailableReason, DimSentVia, DimContractCancelledReason | Extracts global picklist labels and codes for various CRM enums, mapping Option/LocalizedLabel to dimension keys/values. |
| OptionsetMetadata       | DimFacilityEligible, DimAttorneyClassification, DimVaccineType, DimWebCaseType, DimAlternateIDVerification, DimRecoveryExperience, DimReviewType, DimRiskRanking, DimReasonforReferral, DimPayoffTracking, DimPricingChanged, DimReasonforRequest, DimContractType, DimLetterRecipientType                                                                                                                                                                                                                                                                                                                                                                                               | Pulls entity-specific picklist metadata, linking options to descriptive dimensions.                                      |
| StatusMetadata          | DimFundingStatus, DimLeadStatusCode, DimOpportunityStatusCode, DimUWStatusCode, DimCasePartyStatusCode, DimAppointmentStatusCode, DimBGStatusCode, DimOpportunitiesStatusCode                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | Retrieves status codes and labels, upserting into status-related dimensions.                                             |
| account                 | DimFacility                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              | Filters for facility accounts (firmtype=100000000), mapping IDs/names to facility dim.                                   |
| drb_subcase             | DimSubCaseType                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           | Extracts sub-case IDs/names for sub-case type dimension.                                                                 |
| systemuser              | DimEmployees                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             | Maps user IDs/full names to employee/owner dimension.                                                                    |


### 2. import_activty (Daily and weekends))
The "import_activty" pipeline orchestrates three sequential data flows (df_activtyparty, df_connectionrole, df_usc_writeoff) to extract, filter, transform, and upsert recent activity-related records from Dataverse tables into SQL Server CRM tables, focusing on incremental loads based on modification dates for entities like activity parties, emails, connection roles, and write-offs.

### Entities Involved Per Flow
| Flow/Activity          | Source Entity          | Sink Table              | Purpose/Link |
|------------------------|------------------------|-------------------------|-------------|
| activityparty (df_activtyparty) | activityparty (for activity parties), email (for emails) | CRM.activityparty (for parties), CRM.email (for emails) | Filters recent (last day) activity parties linked to contacts/leads and emails regarding contacts/leads/opportunities; derives timestamps, selects key columns, and upserts to maintain activity relationships. |
| connection_role (df_connectionrole) | connectionrole | CRM.connectionrole | Filters recent (last day) connection roles, derives createdon timestamp, and upserts all relevant fields to sync role metadata. |
| writeoff (df_usc_writeoff) | usc_writeoff | CRM.usc_writeoff | Filters recent (last day) write-off records, derives createdon timestamp, and upserts financial/approval details for write-off processes. |


The pipeline executes sequentially: it starts with the "activityparty" data flow, sourcing from Dataverse's activityparty and email tables, filtering records modified in the last day and linked to specific entity types (e.g., contacts, leads, opportunities), deriving formatted createdon timestamps from string dates, selecting essential columns like IDs, statuses, and descriptions, altering rows for upserts based on IDs, and sinking to SQL Server's CRM.activityparty and CRM.email tables with error handling for rejected data; upon success, it proceeds to "connection_role", sourcing from connectionrole, applying similar recent filters and timestamp derivations before upserting to CRM.connectionrole; finally, "writeoff" sources from usc_writeoff, filters recent changes, derives timestamps, and upserts detailed write-off data (e.g., approvals, amounts) to CRM.usc_writeoff, ensuring incremental synchronization of activity and related CRM entities for analytics or operational use.

### 3. PRP 

The "PRP" pipeline in Azure Data Factory executes a single data flow ("PRP_Runs") to extract, transform, and load data from Dataverse entities related to PRP (likely Payment Recovery Process) deals, transactions, and batches into SQL Server tables, performing incremental filters on batches, timestamp derivations, and truncates/upserts for data warehousing.
![[Pasted image 20251024202130.png]]
### Entities Involved Per Flow

|Source Entity|Sink Table|Purpose/Link|
|---|---|---|
|encodian_prpdeal|dbo.PRP_Deals|Extracts deal financials (e.g., amounts, percentages), derives timestamps, and loads via truncate/insert for warehousing PRP deal records.|
|encodian_prptransaction|dbo.PRP_Transactions|Pulls transaction details (e.g., allocations, variances), derives timestamps, and inserts after truncate to sync PRP transaction data.|
|encodian_prpbatch|dbo.PRP_Batch|Filters recent modifications (last day), derives timestamps, and upserts batch metadata (e.g., dates, IDs) for PRP batch tracking.|


The pipeline runs a single "RunPRP" activity using the "PRP_Runs" data flow, sourcing from Dataverse tables encodian_prpdeal, encodian_prptransaction, and encodian_prpbatch, deriving proper timestamp formats for createdon from string dates across all streams, filtering only recently modified (last day) batch records while processing all deals and transactions, altering rows for upserts on batches based on IDs, and sinking to SQL Server: truncating and inserting into PRP_Deals and PRP_Transactions with selected columns like amounts, names, and dates, while upserting to PRP_Batch with error handling for all errors, batch commits, and rejected data output, enabling efficient warehousing of PRP-related financial and operational data for analysis.

## 4. Import CMS (Daily and weekends)

This Azure Synapse pipeline sequences ETL syncs from Dataverse (CRM via Synapse) to SQL DW, using incremental upserts for legal/financial entities. Linear chain: main (core) → financials (transactions) → actions (activities) → misc (logs/misc) → DeleteLog (soft-delete auditing) → UpdateDW (triggers final loads). Runs on "IR-Imports" IR with retries, error tolerance, schema drift support, and concurrency=6 for daily CRM-DW alignment.

### Entities by Data Flow

#### Main (df_cms_main)

|Source (Dataverse)|Target (SQL)|Key|Purpose|
|---|---|---|---|
|account, contact, lead, opportunity|CRM.*|Id|Core entities & leads|
|usc_underwriterevaluation, usc_backgroundevaluation|CRM.*|Id|Evaluations & checks|
|drb_case, drb_partyinsurancecompany, drb_representation, connection|CRM.*|Id|Cases, insurance, reps, relations|

#### Financials (df_cms_financials)

|Source (Dataverse)|Target (SQL)|Key|Purpose|
|---|---|---|---|
|drb_contractdetails, drb_moneynetworkcard, drb_portfolio, drb_pricing, drb_funding, drb_payment, drb_pricingperiod, usc_scheduledinstallment|CRM.*|Id|Contracts, funding, payments, schedules|

#### Actions (df_cms_actions)

|Source (Dataverse)|Target (SQL)|Key|Purpose|
|---|---|---|---|
|annotation, appointment, task, list, listmember, queueitem|CRM.*|Id|Notes, meetings, tasks, lists, queues|

#### Misc (df_cms_misc)

|Source (Dataverse)|Target (SQL)|Key|Purpose|
|---|---|---|---|
|phonecall + misc/custom|CRM.*|Id|Calls & custom fields|

#### DeleteLog (df_cms_delete_log)

|Source|Target|Key|Purpose|
|---|---|---|---|
|Cross-entity IDs|dbo.DeleteLog|RecordId|Audit deletions via ID checks|

#### UpdateDW (DWLoad)

- Triggers downstream DW load Stored Procedure


## 5. DWLoad

Calls a stored Procedure, runs after each cms import pipeline