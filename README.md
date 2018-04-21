# Production Grade CloudFormation Templates

### [Three-zone VPC](docs/vpc.md)
* Creates a three-zone VPC.
* Each zone has both a private and public subnet.
* VPC and subnet CIDRs are customizable.
* Can disable outbound internet access for the public and/or private subnets.
* Public and private routing is automatically set up.
* NACL rules for HTTP/80, HTTPS/443, and response and return traffic on ephemeral ports.
* Support for custom DHCP options.
* Optional S3 endpoint.
* Optional DynamoDB endpoint.
* [More](docs/vpc.md)

### [Route53 Private Zone](docs/private-zone.md)
* Creates a private Route53 hosted zone.
* Associate up to 5 VPCs. CIDRs must not overlap.

### [Software VPN Instance](docs/vpn.md)
* Creates a single `OpenVPN` instance in the first public subnet of the specified VPC.
* Uses exported outputs of the specified VPC stack to get VPC values.
* Attaches a new EIP, and creates a Security Group with all the needed rules.
* Adds required NACL rules to the VPC.
* Creates a Route53 entry for the VPN instance.
