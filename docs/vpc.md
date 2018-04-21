# Three-zone Virtual Private Cloud ([vpc.yaml](../vpc.yaml))
* Creates a three-zone VPC.
* Each zone has both a private and public subnet.
* VPC and subnet CIDRs are customizable.
* Can disable outbound internet access for the public and/or private subnets.
* Public and private routing is automatically set up.
* NACL rules for HTTP/80, HTTPS/443, and response and return traffic on ephemeral ports.
* Support for custom DHCP options.
* Optional S3 endpoint.
* Optional DynamoDB endpoint.

### Usage
* Use the [vpc.yaml](../vpc.yaml) template to create VPCs in multiple regions.
* See the [suggested multi-region setup](#suggested-multi-region-setup).

### Parameters
* **Stack name:** E.g., `vpc-corp` or `vpc-nonprod` or `vpc-prod`
* **VPC CIDR:** The IPv4 CIDR for the VPC. Must be part of the 10.0.0.0/8 CIDR.
* **Auto-assign public IPs:** Assigns public IP addresses to instances in the public subnets.
* **Create S3 endpoint*:** Creates an S3 endpoint and adds it to the route tables for the public and private zones.
* **Create DynamoDB endpoint:** Creates a DynamoDB enpoint and adds it to the route tables for the public and private zones.
* **Enable DNS support:** Enables the Amazon provided DNS server.
* **Enable DNS host names:** Assign Amazon provided DNS names to instances.
* **Domain name:** A custom domain for private DNS names. Must also specify Name Servers.
* **Name servers:** Up to four IP addresses separated by commas.
* **NTP servers:** Up to four IP addresses separated by commas. Leave empty to use AWS NTP servers.
* **Pubic subnet 1:** IPv4 CIDR for the first public subnet.
* **Pubic subnet 2:** IPv4 CIDR for the second public subnet.
* **Pubic subnet 3:** IPv4 CIDR for the third public subnet.
* **Internet access:** Allow HTTP and HTTPS requests to the internet from the public subnets.
* **Private Subnet 1:** IPv4 CIDR for the first private subnet.
* **Private Subnet 2:** IPv4 CIDR for the second private subnet.
* **Private Subnet 3:** IPv4 CIDR for the third private subnet.
* **Internet access:** Allow HTTP and HTTPS requests to the internet from the private subnets.

# Suggested Multi-region Setup
* VPCs are all part of the same network.
* IP spaces do not overlap.
* Public subnets have 4,094 IPs each for a total of 12,282 in each region.
* Private subnets have 16,382 IP each for a total of 49,146 in each region.
* Each VPC has 4,106 IPs available for additional subnetting.
* Customize CIDRs if you want to use a different IP space.

## Northern Virginia: us-east-1
### Corporate VPC: 10.0.0.0/16
This VPC is used as corporate network. It is where the VPN is located along with any other external or internal corporate tools.
* private1: 10.0.0.0/18
* private2: 10.0.64.0/18
* private3: 10.0.128.0/18

* public1: 10.0.192.0/20
* public2: 10.0.208.0/20
* public3: 10.0.224.0/20

### Nonprod VPC: 10.1.0.0/16
This VPC is used for all nonprod environments. E.g., dev, qa, ancillary.
* private1: 10.1.0.0/18
* private2: 10.1.64.0/18
* private3: 10.1.128.0/18

* public1: 10.1.192.0/20
* public2: 10.1.208.0/20
* public3: 10.1.224.0/20

### Production VPC: 10.2.0.0/16
Used for all production environments. E.g., stage, prod, prodhoc
* private1: 10.2.0.0/18
* private2: 10.2.64.0/18
* private3: 10.2.128.0/18

* public1: 10.2.192.0/20
* public2: 10.2.208.0/20
* public3: 10.2.224.0/20

## Oregon: us-west-2
### Production VPC: 10.3.0.0/16
* private1: 10.3.0.0/18
* private2: 10.3.64.0/18
* private3: 10.3.128.0/18

* public1: 10.3.192.0/20
* public2: 10.3.208.0/20
* public3: 10.3.224.0/20

## Ireland: eu-west-1
### Production VPC: 10.4.0.0/16
* private1: 10.4.0.0/18
* private2: 10.4.64.0/18
* private3: 10.4.128.0/18

* public1: 10.4.192.0/20
* public2: 10.4.208.0/20
* public3: 10.4.224.0/20

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
