---
title: Microsoft Azure Well-Architected Framework
titleSuffix: Azure Architecture Center
description: Azure Well-Architected Framework describes five pillars of cost optimization, operational excellence, performance efficiency, reliability, and security, that result in a high quality and scalable cloud architecture.
author: david-stanford 
ms.author: pnp
ms.date: 11/20/2019
ms.topic: conceptual
ms.service: architecture-center
ms.subservice: well-architected
ms.custom:
  - seojan19
  - guide
  - seo-aac-fy21q3
keywords:
  - "Well-architected framework"
  - "Azure Well Architected Framework"
  - "Azure architecture"
  - "architecture framework"
---

# Microsoft Azure Well-Architected Framework

The Azure Well-Architected Framework is a set of guiding tenets that can be used to improve the quality of a workload. The framework consists of five pillars of architecture excellence: Cost Optimization, Operational Excellence, Performance Efficiency, Reliability, and Security. Incorporating these pillars helps produce a high quality, stable, and efficient cloud architectures.

To assess your workload using the tenets found in the Microsoft Azure Well-Architected Framework, see the [Microsoft Azure Well-Architected Review](/assessments/?id=azure-architecture-review&mode=pre-assessment).  

We also recommend you use Azure Advisor and Advisor Score to identify and prioritize opportunities to improve the posture of your workloads.  Both services are free to all Azure users and align to the five pillars of the Well-Architected Framework:

- __[Azure Advisor](/azure/advisor/)__ is a personalized cloud consultant that helps you follow best practices to optimize your Azure deployments. It analyzes your resource configuration and usage telemetry and then recommends solutions that can help you improve the cost effectiveness, performance, reliability, operational excellence, and security of your Azure resources. Learn more about Azure Advisor [here](/azure/advisor/).

- __[Advisor Score](/azure/advisor/azure-advisor-score)__ is a core feature of Azure Advisor that aggregates Advisor recommendations into a simple, actionable score.  This enables you to tell at a glance if you're taking the necessary steps to build reliable, secure, and cost-efficient solutions, and to prioritize the actions that will yield the biggest improvement to the posture of your workloads. The Advisor score consists of an overall score, which can be further broken down into five category scores corresponding to each of the Well-Architected pillars. Learn more about Advisor Score [here](/azure/advisor/azure-advisor-score).

| Pillar | Description |
|--------|-------------|
| [Cost Optimization][cost-pillar] | Managing costs to maximize the value delivered. |
| [Operational Excellence][devops-pillar] | Operations processes that keep a system running in production. |
| [Performance Efficiency][scalability-pillar] | The ability of a system to adapt to changes in load. |
| [Reliability][resiliency-pillar] | The ability of a system to recover from failures and continue to function. |
| [Security][security-pillar] | Protecting applications and data from threats. |

## Cost Optimization

When you are designing a cloud solution, focus on generating incremental value early. Apply the principles of **[Build-Measure-Learn](/azure/cloud-adoption-framework/innovate/considerations/)**, to accelerate your time to market while avoiding capital-intensive solutions. Use the pay-as-you-go strategy for your architecture, and invest in scaling out, rather than delivering a large investment first version. Consider opportunity costs in your architecture, and the balance between first mover advantage versus "fast follow". Use the cost calculators to estimate the initial cost and operational costs. Finally, establish policies, budgets, and controls that set cost limits for your solution.

### Cost guidance

- Review [cost principles](./cost/overview.md)
- [Develop a cost model](./cost/design-model.md)
- Create [budgets and alerts](./cost/monitor-alert.md)
- Review the [cost optimization checklist](./cost/optimize-checklist.md)

## Operational Excellence

This pillar covers the operations and processes that keep an application running in production. Deployments must be reliable and predictable. They should be automated to reduce the chance of human error. They should be a fast and routine process, so they don't slow down the release of new features or bug fixes. Equally important, you must be able to quickly roll back or roll forward if an update has problems.

Monitoring and diagnostics are crucial. Cloud applications run in a remote data-center where you do not have full control of the infrastructure or, in some cases, the operating system. In a large application, it's not practical to log into VMs to troubleshoot an issue or sift through log files. With PaaS services, there may not even be a dedicated VM to log into. Monitoring and diagnostics give insight into the system, so that you know when and where failures occur. All systems must be observable. Use a common and consistent logging schema that lets you correlate events across systems.

The monitoring and diagnostics process has several distinct phases:

- Instrumentation. Generating the raw data, from application logs, web server logs, diagnostics built into the Azure platform, and other sources.
- Collection and storage. Consolidating the data into one place.
- Analysis and diagnosis. To troubleshoot issues and see the overall health.
- Visualization and alerts. Using telemetry data to spot trends or alert the operations team.

Enforcing resource-level rules via [Azure Policy](/azure/governance/policy/overview) helps ensure adoption of operational excellence best practices for all the assets which support your workload. For example, Azure Policy can help ensure that all of the VMs supporting your workload adhere to a pre-approved list of VM Skus. Azure Advisor provides [a set of Azure Policy recommendations](/azure/advisor/advisor-operational-excellence-recommendations#use-azure-policy-recommendations) to help you quickly identify opportunities to implement Azure Policy best practices for your workload.

Use the [DevOps checklist][devops-checklist] to review your design from a management and DevOps standpoint.

### Operational excellence guidance

- [Design patterns for operational excellence](./devops/devops-patterns.md)
- Best practices: [Monitoring and diagnostics][monitoring]

## Performance efficiency

Performance efficiency is the ability of your workload to scale to meet the demands placed on it by users in an efficient manner. The main ways to achieve this are by using scaling appropriately and implementing PaaS offerings that have scaling built in.

There are two main ways that an application can scale. Vertical scaling (scaling *up*) means increasing the capacity of a resource, for example by using a larger VM size. Horizontal scaling (scaling *out*) is adding new instances of a resource, such as VMs or database replicas.

Horizontal scaling has significant advantages over vertical scaling:

- True cloud scale. Applications can be designed to run on hundreds or even thousands of nodes, reaching scales that are not possible on a single node.
- Horizontal scale is elastic. You can add more instances if load increases, or remove them during quieter periods.
- Scaling out can be triggered automatically, either on a schedule or in response to changes in load.
- Scaling out may be cheaper than scaling up. Running several small VMs can cost less than a single large VM.
- Horizontal scaling can also improve resiliency, by adding redundancy. If an instance goes down, the application keeps running.

An advantage of vertical scaling is that you can do it without making any changes to the application. But at some point you'll hit a limit, where you can't scale any up any more. At that point, any further scaling must be horizontal.

Horizontal scale must be designed into the system. For example, you can scale out VMs by placing them behind a load balancer. But each VM in the pool must be able to handle any client request, so the application must be stateless or store state externally (say, in a distributed cache). Managed PaaS services often have horizontal scaling and autoscaling built in. The ease of scaling these services is a major advantage of using PaaS services.

Just adding more instances doesn't mean an application will scale, however. It might simply push the bottleneck somewhere else. For example, if you scale a web front end to handle more client requests, that might trigger lock contentions in the database. You would then need to consider additional measures, such as optimistic concurrency or data partitioning, to enable more throughput to the database.

Always conduct performance and load testing to find these potential bottlenecks. The stateful parts of a system, such as databases, are the most common cause of bottlenecks, and require careful design to scale horizontally. Resolving one bottleneck may reveal other bottlenecks elsewhere.

Use the [Performance efficiency checklist](scalability/performance-efficiency.md) to review your design from a scalability standpoint.

### Performance efficiency guidance

- [Design patterns for performance efficiency](./scalability/performance-efficiency-patterns.md)
- Best practices: [Autoscaling][autoscale], [Background jobs][background-jobs], [Caching][caching], [CDN][cdn], [Data partitioning][data-partitioning]

## Reliability

A reliable workload is one that is both resilient and available. Resiliency is the ability of the system to recover from failures and continue to function. The goal of resiliency is to return the application to a fully functioning state after a failure occurs. Availability is whether your users can access your workload when they need to.

In traditional application development, there has been a focus on increasing the mean time between failures (MTBF). Effort was spent trying to prevent the system from failing. In cloud computing, a different mindset is required, due to several factors:

- Distributed systems are complex, and a failure at one point can potentially cascade throughout the system.
- Costs for cloud environments are kept low through the use of commodity hardware, so occasional hardware failures must be expected.
- Applications often depend on external services, which may become temporarily unavailable or throttle high-volume users.
- Today's users expect an application to be available 24/7 without ever going offline.

All of these factors mean that cloud applications must be designed to expect occasional failures and recover from them. Azure has many resiliency features already built into the platform. For example:

- Azure Storage, SQL Database, and Cosmos DB all provide built-in data replication, both within a region and across regions.
- Azure managed disks are automatically placed in different storage scale units to limit the effects of hardware failures.
- VMs in an availability set are spread across several fault domains. A fault domain is a group of VMs that share a common power source and network switch. Spreading VMs across fault domains limits the impact of physical hardware failures, network outages, or power interruptions.

That said, you still need to build resiliency into your application. Resiliency strategies can be applied at all levels of the architecture. Some mitigations are more tactical in nature &mdash; for example, retrying a remote call after a transient network failure. Other mitigations are more strategic, such as failing over the entire application to a secondary region. Tactical mitigations can make a big difference. While it's rare for an entire region to experience a disruption, transient problems such as network congestion are more common &mdash; so target these first. Having the right monitoring and diagnostics is also important, both to detect failures when they happen, and to find the root causes.

When designing an application to be resilient, you must understand your availability requirements. How much downtime is acceptable? This is partly a function of cost. How much will potential downtime cost your business? How much should you invest in making the application highly available?

### Reliability guidance

- [Designing reliable Azure applications][resiliency]
- [Design patterns for resiliency](./resiliency/reliability-patterns.md)
- Best practices: [Transient fault handling][transient-fault-handling], [Retry guidance for specific services][retry-service-specific]

## Security

Think about security throughout the entire lifecycle of an application, from design and implementation to deployment and operations. The Azure platform provides protections against a variety of threats, such as network intrusion and DDoS attacks. But you still need to build security into your application and into your DevOps processes.

Here are some broad security areas to consider.

### Identity management

Consider using Azure Active Directory (Azure AD) to authenticate and authorize users. Azure AD is a fully managed identity and access management service. You can use it to create domains that exist purely on Azure, or integrate with your on-premises Active Directory identities. Azure AD also integrates with Office365, Dynamics CRM Online, and many third-party SaaS applications. For consumer-facing applications, Azure Active Directory B2C lets users authenticate with their existing social accounts (such as Facebook, Google, or LinkedIn), or create a new user account that is managed by Azure AD.

If you want to integrate an on-premises Active Directory environment with an Azure network, several approaches are possible, depending on your requirements. For more information, see our [Identity Management][identity-ref-arch] reference architectures.

### Protecting your infrastructure

Control access to the Azure resources that you deploy. Every Azure subscription has a [trust relationship][ad-subscriptions] with an Azure AD tenant.
Use [Azure role-based access control (Azure RBAC)][rbac] to grant users within your organization the correct permissions to Azure resources. Grant access by assigning Azure roles to users or groups at a certain scope. The scope can be a subscription, a resource group, or a single resource. [Audit][resource-manager-auditing] all changes to infrastructure.

### Application security

In general, the security best practices for application development still apply in the cloud. These include things like using SSL everywhere, protecting against CSRF and XSS attacks, preventing SQL injection attacks, and so on.

Cloud applications often use managed services that have access keys. Never check these into source control. Consider storing application secrets in Azure Key Vault.

### Data sovereignty and encryption

Make sure that your data remains in the correct geopolitical zone when using Azure data services. Azure's geo-replicated storage uses the concept of a [paired region][paired-region] in the same geopolitical region.

Use Key Vault to safeguard cryptographic keys and secrets. By using Key Vault, you can encrypt keys and secrets by using keys that are protected by hardware security modules (HSMs). Many Azure storage and DB services support data encryption at rest, including [Azure Storage][storage-encryption], [Azure SQL Database][sql-db-encryption], [Azure Synapse Analytics][data-warehouse-encryption], and [Cosmos DB][cosmos-db-encryption].

### Security resources

- [Azure Security Center][security-center] provides integrated security monitoring and policy management for your workload.
- [Azure Security Documentation][security-documentation]
- [Microsoft Trust Center][trust-center]

<!-- links -->

[identity-ref-arch]: ../reference-architectures/identity/index.yml
[resiliency]: ../framework/resiliency/overview.md
[ad-subscriptions]: /azure/active-directory/active-directory-how-subscriptions-associated-directory
[data-warehouse-encryption]: /azure/data-lake-store/data-lake-store-security-overview#data-protection
[cosmos-db-encryption]: /azure/cosmos-db/database-security
[rbac]: /azure/role-based-access-control/overview
[paired-region]: /azure/best-practices-availability-paired-regions
[resource-manager-auditing]: /azure/azure-resource-manager/resource-group-audit
[security-center]: https://azure.microsoft.com/services/security-center
[security-documentation]: /azure/security
[sql-db-encryption]: /azure/sql-database/sql-database-always-encrypted-azure-key-vault
[storage-encryption]: /azure/storage/storage-service-encryption
[trust-center]: https://azure.microsoft.com/support/trust-center

<!-- patterns -->
[operational-excellence-patterns]: ./devops/devops-patterns.md
[resiliency-patterns]:/azure/architecture//framework/resiliency/reliability-patterns.md
[performance-efficiency-patterns]: ./scalability/performance-efficiency-patterns.md

<!-- practices -->
[autoscale]: ../best-practices/auto-scaling.md
[background-jobs]: ../best-practices/background-jobs.md
[caching]: ../best-practices/caching.md
[cdn]: ../best-practices/cdn.md
[data-partitioning]: ../best-practices/data-partitioning.md
[monitoring]: ../best-practices/monitoring.md
[cost]: /azure/cost-management/cost-mgt-best-practices
[retry-service-specific]: ../best-practices/retry-service-specific.md
[transient-fault-handling]: ../best-practices/transient-faults.md

<!-- checklist -->
[devops-checklist]: ../checklist/dev-ops.md
[scalability-checklist]: ../checklist/performance-efficiency.md

<!-- pillars -->
[cost-pillar]: ./cost/overview.md
[security-pillar]: ./security/overview.md
[resiliency-pillar]: ./resiliency/overview.md
[scalability-pillar]: ./scalability/overview.md
[devops-pillar]: ./devops/overview.md
