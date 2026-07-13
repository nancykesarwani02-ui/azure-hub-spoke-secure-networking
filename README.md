# azure-hub-spoke-secure-networking
# Secure Azure Workload Access — Hub-Spoke Network Architecture
A hands-on implementation of a hub-spoke virtual network topology in Azure, demonstrating centralized security control, network segmentation, and controlled traffic flow between workloads and shared services.
Based on the Microsoft Learn Guided Project: [Configure secure access to workloads with Azure virtual networking services](https://microsoftlearning.github.io/Configure-secure-access-to-workloads-with-Azure-virtual-networking-services/).
## Architecture
![Architecture Diagram](./architecture-diagram.png)
The design follows the hub-spoke pattern, a common enterprise reference architecture for centralizing shared security and connectivity services while keeping workloads isolated in their own spoke networks.
**hub-vnet (10.0.0.0/16)**
- `AzureFirewallSubnet` (10.0.0.0/26) — hosts Azure Firewall with a public IP for centralized inbound/outbound traffic inspection
- Private DNS Zone (`private.contoso.com`) — internal name resolution shared across peered networks
**app-vnet (10.1.0.0/16)** — spoke network
- `frontend` subnet (10.1.0.0/24) — VM1, grouped under an Application Security Group (`app-frontend-asg`)
- `backend` subnet (10.1.1.0/24) — VM2, protected by a Network Security Group (`app-vnet-nsg`)
- `AzureFirewallSubnet` (10.1.63.0/26) — local firewall instance (`app-vnet-firewall`) with private/public IP
- `app-vnet-firewall-rt` — custom route table forcing egress traffic through the firewall
**Connectivity**
- VNet peering between `app-vnet` and `hub-vnet` (bidirectional, traffic forwarding enabled)
- User-defined routes (UDRs) directing spoke traffic through Azure Firewall rather than default system routes
## What this demonstrates
- **Network segmentation** — frontend/backend workload isolation using subnets, NSGs, and ASGs
- **Zero-trust traffic flow** — no subnet has unrestricted default egress; all traffic is inspected via Azure Firewall
- **Centralized security policy enforcement** — firewall and DNS services centralized in the hub, consumed by spokes via peering
- **Controlled routing** — custom UDRs override default system routes to force traffic through inspection points
- **Private name resolution** — internal DNS zone avoids exposing internal hostnames to public resolvers
## Lab exercises completed
| # | Exercise | Focus |
|---|----------|-------|
| 01 | Create and configure virtual networks | VNet creation, subnetting, peering |
| 02 | Create and configure network security groups | NSG rules, subnet-level filtering |
| 03 | Create and configure Azure Firewall | Centralized traffic inspection, firewall rules |
| 04 | Configure network routing | Custom UDRs, forced tunneling through firewall |
| 05 | Create DNS zones and configure DNS settings | Private DNS zone, virtual network links |
## Notes
This is a self-directed learning project completed as part of ongoing Azure certification prep (AZ-104). Resources were provisioned and torn down in a personal Azure subscription; no production data or environments were involved.
