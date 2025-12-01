[Azure AI Foundry](https://ai.azure.com/?cid=learnDocs) is a unified Azure platform-as-a-service offering for enterprise AI operations, model builders, and application development. This foundation combines production-grade infrastructure with friendly interfaces, enabling developers to focus on building applications rather than managing infrastructure.
he platform provides streamlined management through unified Role-based access control (RBAC), networking, and policies under one Azure resource provider namespace.

# AI Foundry Hub
Azure AI Hub is a resource type that is used in combination with Azure AI Foundry resource type, and is only required for selected use cases. Hub resources provide access to open-source model hosting and fine-tuning capabilities, as well as Azure Machine Learning capabilities, next to capabilities supported by its associated AI Foundry resource.

When you create an AI Hub, an Azure AI Foundry resource is automatically provisioned. Hub resources can be used in [Azure AI Foundry](https://ai.azure.com/?cid=learnDocs) and [Azure Machine Learning studio](https://ml.azure.com/).

A hub shares configurations for a group of projects. All projects in the hub share the same security configurations or business domain.

- **Security** including public network access, customer-managed key encryption, and identity controls. Security settings configured on the hub automatically pass down to each project. A managed virtual network is shared between all projects that share the same hub.
 - **Connections** let you access objects in Azure AI Foundry portal that are managed outside of your hub. For example, uploaded data on an Azure storage account, or model deployments on an existing Azure OpenAI or AI Foundry resource. Optionally use connection to store shared credentials, so developers can implicitly access remote objects during development.
- **Compute and quota allocation** is managed as shared capacity for all projects in Azure AI Foundry portal that share the same hub. This quota includes compute instance as managed cloud-based workstation for an individual. The same user can use a compute instance across projects.
- **Policy** enforced in Azure on the hub scope applies to all projects managed under it.
- **Dependent Azure resources** are set up once per hub and associated projects and used to store artifacts you generate while working in Azure AI Foundry portal such as logs or when uploading data. For more information, see [dependent resources](https://learn.microsoft.com/en-us/azure/ai-foundry/concepts/ai-resources#storage-and-key-vault-dependent-resources).

Hubs let you manage connections to existing Azure OpenAI or Azure AI Foundry resources, so you can use their models and selected customization capabilities in hub-based projects.

## Azure AI Foundry project

An Azure AI Foundry project is where you do most of your development work. You can work with your project in the Azure AI Foundry portal, or use the SDK in your preferred development environment.
Azure AI Foundry projects provide developers with self-serve capabilities to independently create new environments for exploring ideas and building prototypes, while managing data in isolation. Projects act as secure units of isolation and collaboration where agents share file storage, thread storage (conversation history), and search indexes. You can also bring your own Azure resources for compliance and control over sensitive data.