# Single VPN Instance ([vpn-instance.yaml](../vpn-instance.yaml))
* Creates a single `OpenVPN` instance in the first public subnet of the specified VPC.
* Uses exported outputs of the specified [VPC stack](vpc-3az.md) to get VPC values.
* Attaches a new EIP, and creates a Security Group with all the needed rules.
* Adds SSH NACL rules to the VPC's public and private NACL's
* Supports IPv6. If enabled, adds IPv6 SSH rules to the VPC's NACLs.
* Creates a Route53 entry for the VPN instance (requires a Route53 Public Zone).
* Connect up to 500 devices and users. You must accept the license agreement at the [AWS Marketplace](https://aws.amazon.com/marketplace/search/results?x=0&y=0&searchTerms=OpenVPN+access+server&page=1&ref_=nav_search_box).

### Usage
* [Create your VPCs](vpc-3az.md).
* [Create your private hosted zone (optional)](private-zone.md)
* Use the [vpn-instance.yaml](../vpn-instance.yaml) template to create a single VPN instance in the first public zone.
* Follow the simple [configuration instructions](#command-line-configuration).

### Parameters
* **Stack Name:** E.g., `vpn-nonprod` or `vpn-prod`
* **VPC Stack:** The name of the VPC stack where the VPN will be deployed. E.g., `vpc-nonprod` or `vpc-prod`
* **Enable IPv6:** Yes, if you want IPv6 SSH rules added to the VPC's NACLs.
* **Instance Type:** The AWS instance type that will be used.
* **Key Name:** The key that will be used to SSH into the host.
* **Host Name:** The DNS host name of the VPN server. E.g., `vpn.nonprod.us-east-1`
* **Domain Name:** The domain name of the Route53 public hosted zone that will contain the DNS record. E.g., `mycompany.net`
* **IPv4 CIDR:** The IPv4 CIDR allowed to SSH into the VPC's public zones. E.g., `0.0.0.0/0`
* **IPv6 CIDR:** The IPv6 CIDR allowed to SSH into the VPC's public zones. E.g., `::/0`

# Command Line Configuration
* These steps are done after the stack has been created.

### Run the setup wizard
* SSH to the instance. The domain name is the outputs of the stack.

```
$ ssh -i nonprod.pem openvpnas@vpn.nonprod.us-east-1.mycompany.net
```
* **The setup will start automatically.**
* Agree to the license terms.
* Accept the default settings for each parameter.

### Create a password for the `openvpn` user
```
$ sudo bash
# passwd openvpn
Enter new UNIX password:
Retype new UNIX password:
passwd: password updated successfully
```

# Admin UI Configuration
* Go to the Admin UI in a browser. The address is in the outputs of the stack.
  * `https://YOUR_DOMAIN_NAME:943/admin/`
* Log in as the `openvpn` user with the password you created in the previous step.

### Update The Hostname
* Go to `Server Network Settings`
* For `Hostname ot IP Address`, Enter the domain name of the VPN instance. E.g., `vpn.nonprod.us-east-1.mycompany.com`
* Click the `Save Settings` button at the bottom of the page.
* Click the `Update Running Server` button at the top of the page.

### Update DNS Settings
* Go to `VPN Settings`
* Scroll to the `DNS Settings` section.
* Select `Have clients use the same DNS servers as the Access Server host`
* Scroll to `DNS resolution zones`
* For `DNS zones` enter the domain name of the private hosted zone that the VPC is associated with. E.g., `myco.net`. See hosted-zone.md for details on how to add your VPCs to a private hosted zone. If you did not set up a private zone, you can enter the AWS provided private domain name. For `us-east-1` the AWS provided domeain name is `ec2.internal`. For other regions it is `<REGION>.compute.internal`.
* Click the `Save Settings` button at the bottom of the page.
* Click the `Update Running Server` button at the top of the page.

### Create A User
* Go to `User Permissions`
* Enter a new username in the `New Username` field.
* Click `Show` in the `More Settings` column.
* Enter a password in the `Local Password` input box.
* Check the `Allow Auto-login` box.
* Check the box that says `Require user permissions record for VPN access`.
* Click the `Save Settings` button.
* Click the `Update Running Server` button.


### Download The User Configuration File
* Log out of the admin console.
* Go to the Client login page by simply removing `/admin` from the browser's current address.
* This is where you can `Connect` to the VPN, or `Login` to download the OpenVPN Connect Client and/or client configuration files.
* Enter the username and password you created, and select `Login` from the dropdown menu.
* Download the `Yourself (autologin profile)`
* Import the file into a supported VPN client, e.g., Tunnelblick or OpenVPN Connect.

## (Optional) Install The Pre-configured OpenVPN Connect Client
* Optionally you can select `OpenVPN Connect for` your OS. It will come pre-configured for the logged in user.
* **NOTE** I have had issues with DNS working correctly with the MacOS OpenVPN client when it is connected to the VPN. It seems the DNS servers are not pushed to the client. For that reason I use Tunnelblick.
