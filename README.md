# Azure Cloud Security Engineering: A Defense-in-Depth Implementation



## Project Overview
This project demonstrates the design and implementation of a comprehensive, multi-layered security architecture within Microsoft Azure. As a consultant, I followed the Defense-in-Depth strategy to establish robust security controls across nine distinct labs. These labs covered identity management, network segmentation, container security, endpoint protection, and automated threat response. The goal was to build a resilient, zero-trust-aligned environment capable of protecting sensitive financial data while providing full visibility and automated mitigation of security threats.



## Technologies & Tools

| Domain | Technologies & Tools Used |
| :--- | :--- |
| **Identity & Access** | Microsoft Entra ID (Azure AD), Role-Based Access Control (RBAC), Group Management |
| **Networking** | Virtual Networks (VNet), Network Security Groups (NSG), Application Security Groups (ASG), Azure Firewall, User-Defined Routes (UDR) |
| **Compute & Containers**| Azure Virtual Machines, Azure Kubernetes Service (AKS), Azure Container Registry (ACR) |
| **Data Security** | Azure Storage, Virtual Network Service Endpoints |
| **Security Operations**| Microsoft Sentinel (SIEM), Microsoft Defender for Cloud, Azure Monitor, Log Analytics Workspace |
| **Automation & IaC**| Logic App Playbooks, Kusto Query Language (KQL), PowerShell, Azure CLI |


## Project Architecture & Implementation Highlights

My work spanned several critical security domains, each building upon the last to create a holistic defense.

### 1. Identity and Access Management (RBAC)
I established a secure identity foundation by creating administrative tiers and implementing a group-based permissions model.

-   **Task:** Created user accounts and security groups using the Azure Portal, PowerShell, and Azure CLI to represent different organizational roles (e.g., Senior Admin, Service Desk).
-   **Security Logic:** Implemented the **Principle of Least Privilege** by assigning the built-in Virtual Machine Contributor role to the Service Desk group. This confined their permissions to managing VMs only, preventing them from altering network or security settings.

### 2. Network Micro-Segmentation & Perimeter Defense
I designed a network structure to prevent lateral movement and implemented a centralized gateway to govern all outbound traffic.

-   **Task:** Deployed a Virtual Network with segregated subnets and used Application Security Groups (ASG) to label servers by function (Web vs. Management). All outbound traffic was forced through an Azure Firewall via a custom User-Defined Route (UDR).
-   **Security Logic:** Configured Azure Firewall **Application Rules** to restrict internet access to only approved FQDNs (e.g., `www.bing.com`), implicitly denying all other outbound traffic. This prevents compromised servers from communicating with malicious C2 infrastructure.

### 3. Securing Modern Workloads (Containers & Kubernetes)
I secured the container pipeline from image storage to runtime execution.

-   **Task:** Deployed a private Azure Container Registry (ACR) to store a custom Nginx image and an Azure Kubernetes Service (AKS) cluster to run the application.
-   **Security Logic:** Integrated AKS with ACR using an **AcrPull role assignment** on the cluster's managed identity. This ensures that only the authorized AKS cluster can pull software images, preventing supply chain attacks and unauthorized access to proprietary application code.

### 4. Data Isolation with Service Endpoints
I protected sensitive data at rest by removing its public network exposure.

-   **Task:** Configured an Azure Storage account and used Virtual Network Service Endpoints to lock it down.
-   **Security Logic:** The storage account's firewall was configured to **block all internet traffic** and only allow access from the private subnet. This was validated by confirming that the storage was unreachable from a public VM, even with valid credentials.

### 5. Advanced Endpoint Protection & JIT Access
I reduced the attack surface of critical virtual machines using Microsoft Defender for Cloud.

-   **Task:** Enabled **Microsoft Defender for Servers Plan 2** and configured **Just-in-Time (JIT) VM Access** for the management server.
-   **Security Logic:** JIT Access closes high-risk ports like RDP (3389) by default. Access is only granted on-demand to pre-approved IP addresses for a limited time, drastically reducing exposure to brute-force attacks and unauthorized login attempts.

### 6. Automated Security Operations (SIEM/SOAR)
I established a SOC "brain" to detect and automatically react to threats in real time.

-   **Task:** Deployed Microsoft Sentinel and connected Azure Activity logs. I then created a custom Analytics Rule and an automated **Logic App Playbook**.
-   **Security Logic & Result:** The Analytics Rule successfully detected the deletion of a JIT access policy. This triggered the Playbook, which automatically escalated the Sentinel Incident severity from "Low" to "Medium" and posted an alert to a Microsoft Teams channel, demonstrating a functional SOAR workflow.




## Project Outcomes & Key Achievements

-   **Established a Hardened Cloud Environment:** Implemented a zero-trust-aligned architecture following industry-standard benchmarks.
-   **Reduced Attack Surface:** Drastically minimized VM exposure using Just-in-Time access and network micro-segmentation.
-   **Prevented Data Exfiltration:** Eliminated public data exposure by locking down Azure Storage with Service Endpoints and controlling egress traffic with Azure Firewall.
-   **Improved Response Time:** Reduced Mean Time to Respond (MTTR) for specific threats by implementing automated Logic App Playbooks for incident triage and notification.


## References
-   [AZ-500 Microsoft Learning GitHub Repository](https://github.com/MicrosoftLearning/AZ500-Azure-Security-Technologies)
-   [NIST Cybersecurity Framework](https://www.nist.gov/cyberframework)
-   [Microsoft Azure Security Documentation](https://docs.microsoft.com/en-us/azure/security/)
