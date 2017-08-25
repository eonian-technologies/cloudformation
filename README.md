# Production Grade CloudFormation Templates

### [Three-zone Virtual Private Cloud](docs/vpc-3az.md)
* Creates a three-zone VPC.
* Each AZ has both a private and public subnet.
* Customizable CIDRs.
* NAT Gateways are set up in each public zone.
* Public and private routing is automatically set up.
* Sensible NACL rules for HTTP/80, HTTPS/443, and ephemeral/high ports.
* Support for IPv6. Adds an Egress-only Internet Gateway, and adds appropriate routing and NACL rules.
* Support for custom DHCP options. TODO: Automatically add routing and NACLs for UDP and TCP 53.

### [Route53 Private Zone](docs/private-zone.md)
* Creates a private Route53 hosted zone.
* Associate up to 5 VPCs. CIDRs must not overlap.

### [Software VPN Instance](docs/vpn-instance.md)
* Creates a single `OpenVPN` instance in the first public subnet of the specified VPC.
* Uses exported outputs of the specified VPC stack to get VPC values.
* Attaches a new EIP, and creates a Security Group with all the needed rules.
* Adds SSH NACL rules to the VPC's public and private NACL's
* Supports IPv6. If enabled, adds IPv6 SSH rules to the VPC's NACLs.
* Creates a Route53 entry for the VPN instance.
