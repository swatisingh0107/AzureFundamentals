# AzureFundamentals  
### Course: [Azure Fundamentals](https://docs.microsoft.com/en-us/learn/paths/azure-fundamentals)
### Notes from Azure course: Azure Fundamentals  
# Create a virtual machine
**What defines a virtual machine on Azure?**  
A virtual machine is defined by a number of factors, including its size and location. Before you bring up your VM, let's briefly cover what's involved.  

**Size:** A VM's size defines its processor speed, amount of memory, initial amount of storage, and expected network bandwidth. Some sizes even include specialized hardware such as GPUs for heavy graphics rendering and video editing.  
**Region:** Azure is made up of data centers distributed throughout the world. A region is a set of Azure data centers in a named geographic location. Every Azure resource, including virtual machines, is assigned a region. East US and North Europe are examples of regions.  
**Network:** A virtual network is a logically isolated network on Azure. Each virtual machine on Azure is associated with a virtual network. Azure provides cloud-level firewalls for your virtual networks called network security groups.  
**Resource groups:** Virtual machines and other cloud resources are grouped into logical containers called resource groups. Groups are typically used to organize sets of resources that are deployed together as part of an application or service. You refer to a resource group by its name.  

# Creating resources in Azure
Normally, the first thing we'd do is to create a resource group to hold all the things that we need to create. This allows us to administer all the VMs, disks, network interfaces, and other elements that make up our solution as a unit.

# Choosing a location
The free sandbox allows you to create resources in a subset of Azure's global regions. Select a region from the following list when creating any resources:

westus2  
southcentralus  
centralus  
eastus  
westeurope  
southeastasia  
japaneast  
brazilsouth  
australiasoutheast  
centralindia  

# Create a Windows VM
```
az vm create \
  --name myVM \
  --resource-group 2375b4f8-937d-4c92-8d29-6d0384ec1b08 \
  --image Win2016Datacenter \
  --size Standard_DS2_v2 \
  --location eastus \
  --admin-username $USERNAME \
  --admin-password $PASSWORD
  ```
  
  **Output JSON**
  ```
  swati_jadon0107@Azure:~$ az vm create --name myVM --resource-group 2375b4f8-937d-4c92-8d29-6d0384ec1b08 --image Win2016Datacenter --size Standard_DS2_v2 --location eastus --admin-username $USERNAME --admin-password $PASSWORD
{
  "fqdns": "",
  "id": "/subscriptions/457e6ffc-5ba8-4a0a-b13c-7d23ae3053c7/resourceGroups/2375b4f8-937d-4c92-8d29-6d0384ec1b08/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "eastus",
  "macAddress": "00-0D-3A-1D-7F-44",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "40.121.51.88",
  "resourceGroup": "2375b4f8-937d-4c92-8d29-6d0384ec1b08",
  "zones": ""
}
```
  
  **Verify your VM is running**
  ```
  az vm get-instance-view \
  --name myVM \
  --resource-group 2375b4f8-937d-4c92-8d29-6d0384ec1b08 \
  --output table
  ```
  
 # What is IIS?
Internet Information Services, or IIS, is a web server that runs on Windows. You can use IIS to serve standard web content (HTML, CSS, and JavaScript) or run ASP.NET and other kinds of web applications. IIS comes with Windows Server, but you need to activate it to start serving web pages.  

# Configure IIS

You can store your scripts in Azure storage or in a public location such as GitHub. You can run scripts manually or as part of a more automated deployment. Here, you'll run an Azure CLI command to download a PowerShell script from GitHub and execute it on your VM. The script configures IIS.  

```
az vm extension set \
  --resource-group 2375b4f8-937d-4c92-8d29-6d0384ec1b08 \
  --vm-name myVM \
  --name CustomScriptExtension \
  --publisher Microsoft.Compute \
  --settings "{'fileUris':['https://raw.githubusercontent.com/MicrosoftDocs/mslearn-welcome-to-azure/master/configure-iis.ps1']}" \
  --protected-settings "{'commandToExecute': 'powershell -ExecutionPolicy Unrestricted -File configure-iis.ps1'}"
  ```
  
  **Open port 80(HTTP)**
  ```
  az vm open-port \
  --name myVM \
  --resource-group 2375b4f8-937d-4c92-8d29-6d0384ec1b08 \
  --port 80
  ```
  
  **Verify the configuration**  
  ```
  az vm show \
  --name myVM \
  --resource-group 2375b4f8-937d-4c92-8d29-6d0384ec1b08 \
  --show-details \
  --query [publicIps] \
  --output tsv
  ```
  
  **Machine's IP address is 40.121.51.**
  
  # What is scale?
Scale refers to adding network bandwidth, memory, storage, or compute power to achieve better performance.
Scaling up, or **vertical scaling**, means to increase the memory, storage, or compute power on an existing virtual machine. For example, you can add additional memory to a web or database server to make it run faster.  

Scaling out, or **horizontal scaling**, means to add extra virtual machines to power your application. For example, you might create many virtual machines configured in exactly the same way and use a load balancer to distribute work across them.  

```
az vm resize \
  --resource-group 2375b4f8-937d-4c92-8d29-6d0384ec1b08 \
  --name myVM \
  --size Standard_DS3_v2
  ```
  
  ```
  az vm show \
  --resource-group 2375b4f8-937d-4c92-8d29-6d0384ec1b08 \
  --name myVM \
  --query "hardwareProfile" \
  --output tsv
  ```
  # Datacenters and Regions in Azure
  A **region** is a geographical area on the planet containing at least one, but potentially multiple datacenters that are nearby and networked together with a low-latency network. Azure intelligently assigns and controls the resources within each region to ensure workloads are appropriately balanced.  
  **Some services or virtual machine features are only available in certain regions, such as specific virtual machine sizes or storage types. There are also some global Azure services that do not require you to select a particular region, such as Microsoft Azure Active Directory, Microsoft Azure Traffic Manager, and Azure DNS.** 
  
  ### Special Azure Regions
  - US DoD Central, US Gov Virginia, US Gov Iowa and more: These are physical and logical network-isolated instances of Azure for US government agencies and partners. These datacenters are operated by screened US persons and include additional compliance certifications.  

- China East, China North and more: These regions are available through a unique partnership between Microsoft and 21Vianet, whereby Microsoft does not directly maintain the datacenters.

Azure divides the world into geographies that are defined by geopolitical boundaries or country borders. 
- Geographies allow customers with specific data residency and compliance needs to keep their data and applications close.  
- Geographies ensure that data residency, sovereignty, compliance, and resiliency requirements are honored within geographical boundaries.  
- Geographies are fault-tolerant to withstand complete region failure through their connection to dedicated high-capacity networking infrastructure.  

Geographies are broken up into the following areas:  
Americas  
Europe  
Asia Pacific  
Middle East and Africa  

# Availability Zones
Availability Zones are physically separate datacenters within an Azure region.

Each Availability Zone is made up of one or more datacenters equipped with independent power, cooling, and networking. It is set up to be an isolation boundary. If one zone goes down, the other continues working. Availability Zones are connected through high-speed, private fiber-optic networks.  

Supported regions
Not every region has support for Availability Zones. The following regions have a minimum of three separate zones to ensure resiliency.  
Central US  
East US 2  
West US 2  
West Europe  
France Central  
North Europe  
Southeast Asia  

Availability Zones are primarily for VMs, managed disks, load balancers, and SQL databases. Azure services that support Availability Zones fall into two categories:  

**Zonal services** – you pin the resource to a specific zone (for example, virtual machines, managed disks, IP addresses)  
**Zone-redundant services** – platform replicates automatically across zones (for example, zone-redundant storage, SQL Database).  

# Region Pairs
Availability zones are created using one or more datacenters, and there are a minimum of three zones within a single region. However, it's possible that a large enough disaster could cause an outage big enough to affect even two datacenters. That's why Azure also creates region pairs.  

Azure region is always paired with another region within the same geography at least 300 miles away. Helps reduce the likelihood of interruptions due to events such as natural disasters, civil unrest, power outages, or physical network outages affecting both regions at once.

**Additional advantages of region pairs include:**

If there's an extensive Azure outage, one region out of every pair is prioritized to help reduce the time it takes to restore them for applications.  
Planned Azure updates are rolled out to paired regions one region at a time to minimize downtime and risk of application outage.
Data continues to reside within the same geography as its pair (except for Brazil South) for tax and law enforcement jurisdiction purposes.  

# SLAs for Azure products and services
There are three key characteristics of SLAs for Azure products and services:  

1. Performance Targets: The performance targets that an SLA defines are specific to each Azure product and service.     
2. Uptime and Connectivity Guarantees: These targets can apply to such performance criteria as uptime or response times for services.   
3. Service credits: describe how Microsoft will respond if an Azure product or service fails to perform to its governing SLA's specification.  

MONTHLY UPTIME PERCENTAGE |	SERVICE CREDIT PERCENTAGE
--- | --- | ---
< 99.9 |	10
< 99 |	25
< 95 |	100
