**Tenant**: A dedicated playground for the organization with identity and access management.
Control center across organization

**Fabric:** A deployable cloud data service by Microsoft
**Default workspace**: My workspace, workspace consumes capacity, needs to be attached to a capacity.

Need to have different capacities for different environment: i.e. Dev, Stg, Prod.

**Domain**: Logical grouping of sub-domains
**Subdomains**: Logical grouping of workspaces

**Items**: Inside each workspace we can create items, such as notebooks, data stores like lake houses etc...
**Object**: Inside items we can create objects, like in warehouse we can create tables, stored procedures, views.
![[Pasted image 20251016211636.png]]

## One lake

OneLake is the unified data lake for the whole organization, there will be only one OneLake per fabric, it is the SaaS version of ADLS, OneLake is built on top of ADLS, MS make it easier to use without us having to maintain infrastructure of the OneLake.

We can store files and tables, data is stored in **Delta Parquet** format same as Databricks, but it has a slide edge as we can create **Shortcuts** using it. ( Eliminates Data Duplication) essentially making OneLake a single portal for data

We can also mirror data.


***Accessing data in OneLake***
OneLake has a serverless computer layer that is powered by MS Fabric.
Compute and Storage is separated out.
Computer layer provides various interfaces for accessing data like : Spark, KQL, DSQL, ADFSS etc...
On Top of Computer layer is Data consumption layer using tools like data pipeline, PowerBI ETC..
Fabric Data stores actually access data using OneLake.