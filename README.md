## Build load-balanced VM Architecture for scalability and availability

![Infrastructure Diagram](https://docs.microsoft.com/en-us/azure/architecture/reference-architectures/virtual-machines-windows/images/multi-vm-diagram.png)

# Build Steps

1. Create VNET & Subnet (& resource group)
2. Create NSG - Allow RDP, Allow HTTP
3. Edit subnet to specify NSG
4. Create Availability set
5. Create VMs and add to Availability Set
	1. Use storage account, default diagnostics
	2. Don't create a dedicated NSG - set as None
6. Configure VM with IIS roles
7. Add Load balancer, PIP and NAT rules


# VM Configuration
	- Standard DS1_v2 x2
	- Windows 2012-R2-Datacenter
	- Testuser
	- AzureP@55w0rd123
	- No public IP address
	- Dynamic IP address
	- OS Disk readwrite caching
	- 128GB Data disk - no caching
	- Use customscriptextensions to install MSFT AV
	- Use DSC extension to configure IIS
		-Download 
		https://raw.githubusercontent.com/mspnp/reference-architectures/master/virtual-machines/multi-vm/extensions/windows/iisaspnet.ps1.zip
		- Supply as parameter
		- Module-qualified Name - iisaspnet.ps1\iisaspnet
		- Version 2.20
	- Turn on Diagnostics logging
		
# Load Balancer Configuration
	- Public Load balancer
	- Static IP 
	- Allow http
	- Front end/back end 80 - No NAT
	- Set up back end pools to reference availability set and each VM NIC
	- Set up Health probes - port 80 and /
	- Set up load balancing rules, no floating IP
	- Inbound NAT rules - for the VMs, NAT from 3389 (RDP) to 50000 and 50001

# Additional steps
- Go to Azure Monitor and see actions performed against the subscription
- Turn on Network Watcher for Region
- Test the Loadbalancer and failover by stopping one of the VMs
