
# 🧭 AZURE CONCEPT MAP

We’ll structure this into **layers**, each adding one more piece of understanding.

---

## 🪐 LAYER 1 – **Azure Core Building Blocks**

|Concept|Description|Analogy|
|---|---|---|
|**Tenant (Directory)**|Represents your organization’s identity in Azure Active Directory (now called **Microsoft Entra ID**). It holds your users, groups, and permissions.|Like a _company’s ID system_ that authenticates who’s who.|
|**Subscription**|A logical container for Azure resources and billing. Every resource you create lives inside one subscription.|Like a _credit card account_ — all your Azure usage is billed here.|
|**Resource Group**|A folder-like container inside a subscription that holds related resources (VMs, storage, databases, AI projects, etc.).|Like a _project folder_.|
|**Region**|Physical datacenter location (e.g. `eastus`, `westeurope`) where your resources live.|Like choosing which _branch office_ stores your data.|
|**Resource**|The actual thing you use — a VM, Storage Account, Function App, AI Foundry project, etc.|Like the _actual machines and tools_ you’re using.|

---

## ⚙️ LAYER 2 – **Resource Providers & Types**

Azure is modular. Each service (Compute, Storage, ML, AI, etc.) is powered by a **Resource Provider (RP)**.

|Example Provider|Namespace|Resource Types|
|---|---|---|
|**Compute**|`Microsoft.Compute`|`virtualMachines`, `availabilitySets`|
|**Storage**|`Microsoft.Storage`|`storageAccounts`|
|**Web / App Service**|`Microsoft.Web`|`sites`, `serverFarms`|
|**Azure AI Foundry**|`Microsoft.AI`|`projects`, `hubs`|
|**Azure Machine Learning**|`Microsoft.MachineLearningServices`|`workspaces`, `models`, `endpoints`|
|**Azure OpenAI**|`Microsoft.CognitiveServices`|`accounts` (with OpenAI deployment inside)|

You often register them first:

```bash
az provider register --namespace Microsoft.AI
```

---

## 🧩 LAYER 3 – **Resource Hierarchy**

Azure uses this containment hierarchy:

```
Tenant (Microsoft Entra ID)
└── Subscription
    └── Resource Groups
        └── Resources
            └── Resource Types (defined by Resource Providers)
```

Example:

```
Tenant: Contoso Ltd
└── Subscription: Contoso-AI-Prod
    └── Resource Group: RG-Foundry
        └── Resource: my-ai-project (Microsoft.AI/projects)
```

---

## 🔐 LAYER 4 – **Access, Identity, and Governance**

|Concept|Purpose|Example|
|---|---|---|
|**Azure RBAC (Role-Based Access Control)**|Grants granular permissions to users, groups, or apps.|Give “Contributor” role to `RG-Foundry`.|
|**Managed Identity**|Lets an Azure service authenticate to others without credentials.|AI Foundry project accessing a Key Vault securely.|
|**Policy**|Enforces rules across resources (e.g., restrict locations, VM sizes).|Allow only `eastus` region deployments.|
|**Blueprints / ARM / Bicep**|Infrastructure-as-Code tools for repeatable deployments.|Define a Foundry + Storage template in Bicep.|

---

## ☁️ LAYER 5 – **Compute, Storage, and Networking**

|Category|Core Resources|What They Do|
|---|---|---|
|**Compute**|Virtual Machines, App Service, Azure Functions, AKS|Run your applications.|
|**Storage**|Blob, File, Queue, Table|Persist data and objects.|
|**Networking**|Virtual Network, Subnet, Load Balancer, Firewall|Connect and secure your services.|

Everything AI or ML in Azure runs _on top of_ these fundamentals.

---

## 🧠 LAYER 6 – **AI and Machine Learning Services**

Azure offers three main “flavors” of AI:

|Service|Namespace|Purpose|
|---|---|---|
|**Azure OpenAI Service**|`Microsoft.CognitiveServices`|Access GPT-4, GPT-4o, Codex, etc.|
|**Azure AI Foundry**|`Microsoft.AI`|Unified environment to build, orchestrate, and manage AI agents, models, prompt flows, and data connections.|
|**Azure Machine Learning (Azure ML)**|`Microsoft.MachineLearningServices`|Train, fine-tune, and deploy custom ML or open-source models.|
|**Cognitive Services**|`Microsoft.CognitiveServices`|Classic APIs: Vision, Speech, Language, Translator, etc.|

These services often _interconnect_:

- Foundry can deploy or consume ML models.
    
- Foundry can call Azure OpenAI endpoints.
    
- Both can store data in Azure Storage or Azure Search (Cognitive Search / AI Search).
    

---

## 🧩 LAYER 7 – **Automation and DevOps**

|Tool|Purpose|
|---|---|
|**Azure CLI**|Command-line management tool (`az login`, `az group create`, etc.)|
|**Azure PowerShell**|PowerShell module for Azure resource scripting.|
|**ARM Templates / Bicep**|Declarative templates for Infrastructure-as-Code.|
|**Azure DevOps / GitHub Actions**|CI/CD pipelines that deploy your templates and apps.|

Example command:

```bash
az deployment group create \
  --resource-group RG-Foundry \
  --template-file main.bicep
```

---

## 🧭 LAYER 8 – **Monitoring, Security, and Management**

|Service|What It Does|
|---|---|
|**Azure Monitor / Log Analytics**|Metrics, logs, and alerts for all resources.|
|**Application Insights**|Telemetry for apps and APIs.|
|**Defender for Cloud**|Security posture management and threat detection.|
|**Cost Management + Budgets**|Track and control your spending.|

---

## 🧩 LAYER 9 – **AI Foundry Architecture (Modern Layer)**

Inside **Azure AI Foundry**, you’ll see:

|Concept|Meaning|
|---|---|
|**AI Hub**|Org-level container for projects; shares compute, networking, identity.|
|**Foundry Project**|Workspace for building apps, agents, evaluations.|
|**Connections**|Linked data sources (e.g. Blob, Search, Cognitive Services).|
|**Prompt Flow / Agent**|Low-code orchestration tools for LLM pipelines.|

Everything here still lives _under the same Azure resource model_: Tenant → Subscription → Resource Group → Resource.

---

## ⚙️ LAYER 10 – **Putting It All Together (Example)**

Let’s say you build an app that uses GPT-4 to analyze SDS-C document data.

1. **Tenant:** Your organization’s Microsoft Entra ID.
    
2. **Subscription:** `AI-Development`.
    
3. **Resource Group:** `rg-sdsc-ai`.
    
4. **Resources:**
    
    - `Microsoft.AI/hubs` → your AI Hub
        
    - `Microsoft.AI/projects` → your Foundry project
        
    - `Microsoft.CognitiveServices/accounts` → OpenAI endpoint
        
    - `Microsoft.Storage/storageAccounts` → document storage
        
    - `Microsoft.Search/searchServices` → AI Search index
        
5. **User Access:** via RBAC roles in that resource group.
    
6. **Automation:** Use Azure CLI or Bicep to deploy.
    

---

## ✅ Summary (Mental Model)

|Layer|What You Learn|
|---|---|
|1|Azure structure (Tenant → Subscription → RG → Resource)|
|2|Resource providers and namespaces|
|3|Identity, Access, Policy|
|4|Core Infra: Compute, Storage, Network|
|5|AI Services: OpenAI, ML, Foundry|
|6|DevOps: CLI, Bicep, GitHub Actions|
|7|Monitoring, Security, Cost|
|8|Advanced: AI Hub, Foundry Project, Connections|

---

Would you like me to create a **visual diagram (hierarchical map)** that shows all these Azure layers and how AI Foundry fits into the overall Azure ecosystem? It’ll help you memorize and navigate them instantly.