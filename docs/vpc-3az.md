# Three-zone Virtual Private Cloud ([vpc-3az.yaml](../vpc-3az.yaml))
* Creates a three-zone VPC.
* Each AZ has both a private and public subnet.
* Customizable CIDRs.
* NAT Gateways are set up in each public zone.
* Public and private routing is automatically set up.
* Sensible NACL rules for HTTP/80, HTTPS/443, and ephemeral/high ports.
* Support for IPv6. Adds an Egress-only Internet Gateway, and adds appropriate routing and NACL rules.
* Support for custom DHCP options. TODO: Automatically add routing and NACLs for UDP and TCP 53.
* Can be associated with a [private Route53 Hosted Zone](private-zone.md).
* Access instances using a [single VPN instance](vpn-instance.md), or a more robust [high-availability VPN](vpn-ha.md) setup.

### Usage
* Use the [vpc-3az.yaml](../vpc-3az.yaml) template to create VPCs in multiple regions.
* See the [suggested multi-region setup](#suggested-multi-region-setup).

### Parameters
* **Stack name:** E.g., `vpc-nonprod` or `vpc-prod`
* **VPC Type:** The type of VPC. E.g., `nonprod` or `prod`
* **VPC CIDR:** The IPv4 CIDR for the VPC.
* **Enable IPv6:** Adds IPv6 routes and rules to the VPC's route tables and NACLs.
* **Auto-assign public IPs:** Assigns public IP addresses to instances in the public subnets.
* **Enable DNS Support:** Enables the Amazon provided DNS server.
* **Enable DNS Host Names:** Assign Amazon provided DNS names to instances.
* **Domain Name:** A custom domain for private DNS names. Must also specify Name Servers.
* **Name Servers:** Up to four IP addresses separated by commas.
* **NTP Servers:** Up to four IP addresses separated by commas. Leave empty to use AWS NTP servers.
* **Pubic Subnet 1:** IPv4 CIDR for the first public subnet.
* **Pubic Subnet 2:** IPv4 CIDR for the second public subnet.
* **Pubic Subnet 3:** IPv4 CIDR for the third public subnet.
* **Private Subnet 1:** IPv4 CIDR for the first private subnet.
* **Private Subnet 2:** IPv4 CIDR for the second private subnet.
* **Private Subnet 3:** IPv4 CIDR for the third private subnet.

# Suggested Multi-region Setup
* VPCs should be deployed in multiple regions.
* VPCs are part of a larger 10/8 network.
* IP space does not overlap.
* Public subnets have 4,094 IPs each for a total of 12,282 in each region.
* Private subnets have 16,382 IP each for a total of 49,146 in each region.
* Each VPC has 4,106 IPs available for additional subnetting.
* Customize CIDRs if you want to use a different IP space.

## Northern Virginia: us-east-1
### nonprod: 10.0.0.0/16
* private1: 10.0.0.0/18
* private2: 10.0.64.0/18
* private3: 10.0.128.0/18


* public1: 10.0.192.0/20
* public2: 10.0.208.0/20
* public3: 10.0.224.0/20

### prod: 10.1.0.0/16
* private1: 10.1.0.0/18
* private2: 10.1.64.0/18
* private3: 10.1.128.0/18


* public1: 10.1.192.0/20
* public2: 10.1.208.0/20
* public3: 10.1.224.0/20

## Oregon: us-west-2
### prod: 10.2.0.0/16
* private1: 10.2.0.0/18
* private2: 10.2.64.0/18
* private3: 10.2.128.0/18


* public1: 10.2.192.0/20
* public2: 10.2.208.0/20
* public3: 10.2.224.0/20

## Ireland: eu-west-1
### prod: 10.3.0.0/16
* private1: 10.3.0.0/18
* private2: 10.3.64.0/18

# Manual Cleanup
* Done after the VPC is created.
* Done via the AWS Console.

### Route Tables
* Rename the Main Route Table to 'DO-NOT-USE'.
* Ensure the Main Route Table has no routes (other than the default routes).
* Ensure the Main Route Table is not associated with any Subnets.

### DHCP Options Sets
* Ensure the VPC is not using the unnamed DHCP Options Set.
* Delete the unnamed DHCP Options Set.

### Network ACLs
* Rename the 'default' NACL to 'DO-NOT-USE'.
* Remove all Inbound and Outbound Rules from the 'default' NACL.
* Ensure the 'default' NACL has no subnet associations.

### Security Groups
* Rename the 'default' Security Group to 'DO-NOT-USE'.
* Remove all Inbound and Outbound Rules from the 'default' Security Group.

### IPv6 (if enabled)
* Enable auto-assign IPv6 address in public subnets.
