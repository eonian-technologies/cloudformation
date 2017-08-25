# Route53 Private Zone ([private-zone.yaml](../private-zone.yaml))
* Creates a private Route53 hosted zone.
* Associate up to 5 VPCs. CIDRs must not overlap.

### Usage
* Create all your regional VPCs first. See the [suggested multi-region setup](vpc-3az.md#suggested-multi-region-setup).
* Using the [private-zone.yaml](../private-zone.yaml) template, create a single private Route53 Hosted Zone and associate all your VPC.
* Be sure that your VPC CIDRs do not overlap.

### Parameters
* **Name:** The name of the hosted zone. E.g., `mycompany.net`
* **Comment:** Optional comment. E.g., `Internal Only`
* **VPC1Id:** The ID of the first VPC to associate.
* **VPC1Region:** The region of the first VPC.
* **VPC2Id:** The ID of the second VPC.
* **VPC2Region:** The region of the second VPC.
* **VPC3Id:** The ID of the third VPC.
* **VPC3Region:** The region of the third VPC.
* **VPC4Id:** The ID of the fourth VPC.
* **VPC4Region:** The region of the fourth VPC.
* **VPC5Id:** The ID of the fifth VPC.
* **VPC4Region:** The region of the fifth VPC.
