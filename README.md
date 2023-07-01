# Imersão Azure Expert - Turma 4

Realização: Maia Academy e Azure Expert

Este é o repositório que contém o material Online e Hands-on da Imersão.

Para realizar as atividades do Hands-on Lab estamos utilizando o Portal do Azure no idioma Inglês a fim de manter o padrão e não haver erros, e se necessário caso tenha alguma dificuldade no entendimento você pode abrir o [Google Tradutor](https://translate.google.com.br/) e traduzir para melhor entendimento.

### Hands-on Lab
## Day 1

## Exercise 1.1 - Deploy Virtual Networks for Hub (15 minutes)

1. In the Azure portal, search for and select **Virtual networks**, and, on the **Virtual networks** blade, click **+ Add**.

1. Create a virtual network with the following settings (leave others with their default values):

    | Setting | Value |
    | --- | --- |
    | Subscription | the name of the Azure subscription you will be using in this lab |
    | Resource Group | (create new) **WGVNetRG1** |
    | Name | **WGVNet1** |
    | Region | the name of any Azure region available in the subscription you will use in this lab |
    | IPv4 address space | **10.7.0.0/20** |
    | Subnet name | **Management** |
    | Subnet address range | **10.7.2.0/25** |
     | | |
      
1. Accept the defaults and click **Review and Create**. Let validation occur, and hit **Create** again to submit your deployment.

1. Once the deployment completes browse for **Virtual Networks** in the portal search bar. Within **Virtual networks** blade, click on the newly created virtual network.

1. On the virtual network blade, click **Subnets** and then click **+ Subnet**. 

1. Create a subnet with the following settings (leave others with their default values):

    | Setting | Value |
    | --- | --- |
    | Name | **AzureFirewallSubnet** |
    | Address range (CIDR block) | **10.7.1.0/24** |
    | Network security group | **None** |
    | Route table | **None** |
    | | |

1. Repeat create a subnet with the following settings (leave others with their default values):

    | Setting | Value |
    | --- | --- |
    | Name | **AzureBastionSubnet** |
    | Address range (CIDR block) | **10.7.5.0/24** |
    | Network security group | **None** |
    | Route table | **None** |
    | | |


1. Explore properties to Virtual networks.

## Exercise 1.2 - Deploy Virtual Machine for Hub (15 minutes)

1. In the Azure portal, search for and select **Virtual machines** and, on the **Virtual machines** blade, click **+ Add**.

1. On the **Basics** tab of the **Create a virtual machine** blade, specify the following settings (leave others with their default values):

    | Setting | Value | 
    | --- | --- |
    | Subscription | the name of the Azure subscription you will be using in this lab |
    | Resource group | **WGVNetRG1** |
    | Virtual machine name | **WGVM1** |
    | Region | select same region the Resouce group | 
    | Availability options | **No** |
    | Image | **Ubuntu Server 18.04 LTS - Gen1** |
    | Azure Spot instance | **No** |
    | Size | **Standard_B1ms or other** |
    | Authentication type | Password |
    | Username | **admaz** |
    | Password | **Arqu1t3t0Cl0ud*** |
    | Public inbound ports | **SSH (22) and HTTP (80)** |
     | | |

1. Click **Next: Disks >** and, on the **Disks** tab of the **Create a virtual machine** blade, specify the following settings (leave others with their default values):

    | Setting | Value | 
    | --- | --- |
    | OS disk type | **Standard SSD** |
    | Enable Ultra Disk compatibility | **No** |

1. Click **OK** and, back on the **Networking** tab of the **Create a virtual machine** blade, specify the following settings (leave others with their default values):

    | Setting | Value | 
    | --- | --- |
    | Virtual Network | **WGVNet1** |
    | Subnet | **Management** |
    | Public IP | **WGVM1-PI** |
    | NIC network security group | **Basic** |
    | Accelerated networking | **Off** |
	| Inbound Ports | **SSH (22) and HTTP (80)** |
    | Place this virtual machine behind an existing load balancing solution? | **No** |
    | | |

1. Click **Next: Management >** and, on the **Management** tab of the **Create a virtual machine** blade, specify the following settings (leave others with their default values):

    | Setting | Value | 
    | --- | --- |
    | Boot diagnostics | **Enable with managed storage account (recommended)** |
** |
   | Enable auto-shutdown | off |   
    
1. On the **Review + Create** blade, click **Create**.

1. Connect Virtual machine, in the session SSH.

1. In the terminal session, run the following for install NGINX Web Server to **WGVM1**.

   ```shell
   sudo apt-get -y update
   sudo apt-get -y install nginx
   ```

1. Within the computer, start Browser and navigate on **Public IP Address** of the **WGVM1**.

## Exercise 1.3 - Configure Azure DNS for internal name resolution for Hub (15 minutes)

1. In the Azure portal, search for and select **Private DNS zones** and, on the **Private DNS zones** blade, click **+ Add**.

1. Create a private DNS zone with the following settings (leave others with their default values):

    | Setting | Value |
    | --- | --- |
    | Subscription | the name of the Azure subscription you are using in this lab |
    | Resource Group | **WGVNetRG1** |
    | Name | **woodgrove.corp** |

1. Click Review and Create. Let validation occur, and hit Create again to submit your deployment.

    >**Note**: Wait for the private DNS zone to be created. This should take about 2 minutes.

1. Click **Go to resource** to open the **woodgrove.corp** DNS private zone blade.

1. On the **woodgrove.corp** private DNS zone blade, in the **Settings** section, click **Virtual network links**

1. Click **+ Add** to create a virtual network link with the following settings (leave others with their default values):

    | Setting | Value |
    | --- | --- |
    | Link name | **VNL-WGVNet1** |
    | Subscription | the name of the Azure subscription you are using in this lab |
    | Virtual network | **WGVNet1** |
    | Enable auto registration | enabled |

1. Click **OK**.

    >**Note:** Wait for the virtual network link to be created. This should take less than 1 minute.

1. On the **woodgrove.corp** private DNS zone blade, in the sidebar, click **Overview**

1. Verify that the DNS records for **WGVM1** appear in the list of record sets as **Auto registered**.

    >**Note:** You might need to wait a few minutes and refresh the page if the record sets are not listed.

1. Switch to the SSH session to **WGVM1**.

1. In the Terminal console, run the following to test internal name resolution of the **WGVM1** DNS record set in the newly created private DNS zone:

   ```shell
   nslookup WGVM1.woodgrove.corp
   ```

1. Verify that the output of the command includes the private IP address of **WGVM1**.

## Exercise 2.1 - Deploy Virtual Networks and Application for Spoke (30 minutes)

1. In the Azure portal, search for and select **Virtual networks**, and, on the **Virtual networks** blade, click **+ Add**.

1. Create a virtual network with the following settings (leave others with their default values):

    | Setting | Value |
    | --- | --- |
    | Subscription | the name of the Azure subscription you will be using in this lab |
    | Resource Group | (create new) **WGVNetRG2** |
    | Name | **WGVNet2** |
    | Region | the name of any Azure region available in the subscription you will use in this lab |
    | IPv4 address space | **10.8.0.0/20** |
    | Subnet name | **AppSubnet** |
    | Subnet address range | **10.8.0.0/25** |
     | | |
      
1. Accept the defaults and click **Review and Create**. Let validation occur, and hit **Create** again to submit your deployment.

1. Once the deployment completes browse for **Virtual Networks** in the portal search bar. Within **Virtual networks** blade, click on the newly created virtual network.

1. On the virtual network blade, click **Subnets** and then click **+ Subnet**. 

1. Create a subnet with the following settings (leave others with their default values):

    | Setting | Value |
    | --- | --- |
    | Name | **DataSubnet** |
    | Address range (CIDR block) | **10.8.1.0/25** |
    | Network security group | **None** |
    | Route table | **None** |
    | | |

## Exercise 2.2 - Deploy Woodgrove Application for Spoke (30 minutes)

1. Deploy the template **woodgrove.json** to a new resource group. This template deploys a two virtual machines running application and database.

    > **Note:** You can deploy the template by selecting the 'Deploy to Azure' button below. You will need to create a new resource group **WGVNetRG2**. You will also need to select a location **East US or location you hava quotas for VMs**. Then choose **Review + create** followed by **Create**. 

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmaiaacademy%2Fworkshop-day-networking-avancado-azure%2Fmain%2FCloudShop.json" target="_blank">![Button to deploy the Woodgrove template to Azure.](/AllFiles/Images/deploy-to-azure.png)</a>

    > **Note:** The template will take around 30 minutes to deploy. Once template deployment is complete, several additional scripts are executed to bootstrap the lab environment. **Allow at least 45 minutes from the start of template deployment for the scripts to run.**

1. Within the computer, start Browser and navigate to **IP Public address** of the **WGWEB1**.

1. Examine the navegate on Application was successful.

## Exercise 3 - Configure Azure VNET Peering (15 minutes)

1. In the Azure portal, search for and select **Virtual networks**.

1. In the list of virtual networks, click **WGVNet1**.

1. On the **WGVNet2** virtual network blade, in the **Settings** section, click **Peerings** and then click **+ Add**.

1. Specify the following settings (leave others with their default values) and click **Add**:

    | Setting | Value|
    | --- | --- |
    | This virtual network: Peering link name | **WGVNet2-to-WGVNet1** |
    | This virtual network: Traffic to remote virtual network | **Allow (default)** |
    | This virtual network: Traffic forwarded from remote virtual network | **Allow (default)** |
    | Virtual network gateway | **None** |
    | Remote virtual network: Peering link name | **WGVNet1-to-WGVNet2** |    
    | Virtual network deployment model | **Resource manager** |
    | I know my resource ID | unselected |
    | Subscription | the name of the Azure subscription you are using in this lab |
    | Virtual network | **WGVNet1** |
    | Traffic to remote virtual network | **Allow (default)** |
    | Traffic forwarded from remote virtual network | **Allow (default)** |
    | Virtual network gateway | **None** |

1. In the Azure portal, search for **Private DNS Zones** and select **woodgrove.corp**

1. Click **Add** and create a new **Virtual networking links**, on the **WGVNet2**.

1. At the top of the Azure portal, enter the name of a **WGVM1** that is in the running state, in the search box. When the name of the VM appears in the search results, select it.

1. Under Settings on the left, select **Networking**, and navigate to the network interface resource by selecting its name. View network interfaces.

1. On the left, select **Effective routes**. The effective routes for a network interface are shown.
    
1. Connect Virtual Machine on the **WGVNet1**, in the session SSH.

1. In the terminal session, run the following to test connectivity to **WGWEB1**.

   ```shell
   telnet WGWEB1.woodgrove.corp 80
   ```
    >**Note**: The test uses TCP 80 since this is this port is allowed by default by operating system firewall. 

1. Examine the output of the command and verify that the connection was successful.

## Exercise 4 - Network Security groups (30 minutes)

1.  In the Azure portal, select **+ Create a resource**. In the **Search the Marketplace** box, search for and select **Application security group**. Next, on the **Application security group** blade, select **Create**.

2.  On the **Create an application security group** blade, on the **Basics** tab, enter the following information, and select **Review + create**:

    -  Subscription: **Select your subscription**.

    -  Resource group: **WGVNetRG2**

    -  Name: **WebTier**

    -  Region: This must match the location in which you created the **WGVNet2** virtual network.

3.  On the **Create an application security group** blade, on the **Review + Create** tab, ensure the validation passes, and select **Create**. 

4.  Repeat the previous two steps to create an application security group named **DataTier** with the following settings.

    -  Subscription: **Select your subscription**.

    -  Resource group: **WGVNetRG2**

    -  Name: **DataTier**

    -  Region: This must match the location in which you created the **WGVNet2** virtual network.

1.  In the Azure portal, navigate to the **Virtual machines** blade and select **WGWEB1**.

2.  On the **WGWEB1** blade, select **Networking** under **Settings** on the left.

3.  On the **WGWEB1 - Networking** blade, select **Application security groups** and then select **Configure the application security groups**.

4.  On the **Configure the application security groups** blade, in the **Application security groups** drop-down list, select **WebTier**, then **Save**.

6.  Repeat steps, but this time for **WGSQL1** in order to assign to its network interface the **DataTier** application security group.

1.  In the Azure portal, select **+ Create a resource**. In the **Search the Marketplace** box, **Network security group** and press Enter. Select it and on the **Network security group** blade, select **Create**.

2.  On the **Create network security group** blade, enter the following information, and select **Review + Create** then **Create**:
   
    -  Subscription: **Select your subscription**.

    -  Resource group: **WGVNetRG2**

    -  Name: **WGAppNSG1**

    -  Region: This must match the location in which you created the **WGVNet2** virtual network.

3.  In the Azure Portal, navigate to **All Services**, type **Network security groups** the search box and select **Network security groups**.

4.  On the **Network security groups** blade, select **WGAppNSG1**. 

5.  On the **WGAppNSG1** blade, select **Inbound security rules** under **Settings** on the left and select **Add**.

6.  On the **Add inbound security rule** blade, enter the following information, and select **Add**:

    -  Source: **Application security group**

    -  Source application security group: **WebTier**

    -  Source port ranges: **\***

    -  Destination: **Application security group**

    -  Destination application security group: **DataTier**

    -  Destination port ranges: **1433**

    -  Protocol: **TCP**

    -  Action: **Allow**

    -  Priority: **100**

    -  Name: **AllowDataTierInboundTCP1433**

7.  On the **WGAppNSG1 - Inbound security rules** blade, select **Add**.

8.  On the **Add inbound security rule** blade, enter the following information, and select **Add**:

    -  Source: **Any**

    -  Source port ranges: **\***

    -  Destination: **Application security group**

    -  Destination application security group: **WebTier**

    -  Destination port ranges: **80**

    -  Protocol: **TCP**

    -  Action: **Allow**

    -  Priority: **150**

    -  Name: **AllowAnyWebTierInboundTCP80**

9.  On the **WGAppNSG1 - Inbound security rules** blade, select **Add**.

12. On the **Add inbound security rule** blade, enter the following information, and select **Add**:

    -  Source: **Service Tag**

    -  Source service tag: **VirtualNetwork**

    -  Source port ranges: **\***

    -  Destination: **Application security group**

    -  Destination application security group: **DataTier**

    -  Destination port ranges: **\***

    -  Protocol: **Any**

    -  Action: **Deny**

    -  Priority: **1000**

    -  Name: **DenyVNetDataTierInbound**

13. On the **WGAppNSG1 - Inbound security rules** blade, select **Add**.

14. On the **Add inbound security rule** blade, enter the following information, and select **Add**:

    -  Source: **Service Tag**

    -  Source service tag: **VirtualNetwork**

    -  Source port ranges: **\***

    -  Destination: **Application security group**

    -  Destination application security group: **WebTier**

    -  Destination port ranges: **\***

    -  Protocol: **Any**

    -  Action: **Deny**

    -  Priority: **1050**

    -  Name: **DenyVNetWebTierInbound**

15. On the **WGAppNSG1 - Inbound security rules** blade, select **Subnets** under **Settings** and then select **+ Associate**.

16. On the **Associate subnet** blade, select **WGVNet2** on the **Virtual network** drop down **DataSubnet** and **AppSubnet** on the **Subnet** dropdown.

17. Select **OK** at the bottom of the **Associate subnet** blade.

1. Within the computer, start Browser and navigate to **IP Public address** of the **WGWEB1**.

1. Examine the navegate on Application was successful.

## Exercise 5 - Deploy Azure Bastion (15 minutes)

1.  In the Azure portal, select **+ Create a resource** then select **Bastion**. In the search results, select the Bastion service with Microsoft as the publisher.

2.  On the **Create a Bastion** blade, on the **Basics** tab, enter the following information, and select **Review + Create**:

    -  Subscription: **Select your subscription**.

    -  Resource group: Select **WGVnetRG1**.

    -  Name: **AzureBastion**

    -  Region: This must match the location in which you created the **WGVNet1** virtual network.

    - Tier: Basic

    -  Virtual network: **WGVNet1**

    -  Subnet: **AzureBastionSubnet** Note: After creation, assign (10.7.5.0/24) as the subnet address..

    -  Public IP: **Create New**

    -  Public IP address name: **BastionPublicIP**

3.  On the **Create a Bastion** blade, on the **Review + Create** tab, ensure the validation passes, and select **Create**. The Bastion host will take about 15 minutes to provision.

5.  In the Azure portal, navigate to the Network Security groups, navegate on the **WGAppNSG1**, select **Inbound security rules** under **Settings** on the left and select **Add** rules allow RDP/SSH for source **Virtual Network** on TAG and destionation **WebTier and DataTier** on Application Security groups.

1. In the Azure portal, navigate to the **WGWEB1** VM and initiate a Bastion connection session to the WGWEB1 virtual machine by selecting Connect and Bastion, enter the following information:

 - User name: demouser

 - Password demo@pass123

## Exercise 6 - Configure Route Tables (15 minutes)

1.  On the main portal menu, select **+ Create a Resource**. Type **route** into the search box, and select **Route table** then select **Create**.

2.  On the **Create a Route table** blade enter the following information:

    -  Subscription: **Select your subscription**.

    -  Resource group: Select **WGVNetRG1** from the drop down.

    -  Region: This must match the location in which you created the **WGVNet1** virtual network.

    -  Name: **MgmtRT**

    -  Propagate gateway routes: **Yes**

3.  When the dialog looks like the following screenshot, select **Review + Create** then **Create**.

4.  Repeat steps 1 and 2 to create the **AppRT** route table:

    -  Subscription: **Select your subscription**.

    -  Resource group: Select **WGVNetRG2** from the drop down.

    -  Region: This must match the location in which you created the **WGVNet2** virtual network.

    -  Name: **AppRT**

    -  Propagate gateway routes: **Yes**

5.  Once route tables are created, your **Route tables** blade should look like the following screenshot:

1.  Select the **AppRT** route table, and select **Routes** under **Settings** on the left.

2.  On the **Routes** blade, select **+ Add**. Enter the following information, and select **OK**:

    -  Route name: **AppToInternet**

    -  Address prefix: **0.0.0.0/0**

    -  Next hop type: **Virtual appliance**

    -  Next hop address: **10.7.1.4**

3.  Repeat this procedure to add the **AppToMgmt** route using the following information:

    -  Route name: **AppToMgmt**

    -  Address prefix: **10.7.2.0/25**

    -  Next hop type: **Virtual appliance**

    -  Next hop address: **10.7.1.4**

4.  Upon completion, your routes in the **AppRT**.

1. In the Azure Portal, go to All Services and type Route in the search box and select **Route tables**.

6.  Select **MgmtRT**, and select **Routes** under **Settings** on the left.

7.  On the **Routes** blade, select **+Add**. Enter the following information, and select **OK**:

    -  Route name: **MgmtToOnPremises**

    -  Address prefix: **192.168.0.0/16**

    -  Next hop type: **Virtual network gateway**

    -  Next hop address: **Leave blank**.

8.  Add the **MgmtToApp** route using the following information:

    -  Route name: **MgmtToApp**

    -  Address prefix: **10.8.0.0/20**

    -  Next hop type: **Virtual appliance**

    -  Next hop address: **10.7.1.4** (This is the private IP of Azure Firewall.)

9.  Upon completion, your routes in the **MgmtRT**.

5.    >**Note:** The route tables and routes you have just created are not associated with any subnets yet, so they are not impacting any traffic flow yet. This will be accomplished later in the lab.

## Exercise 7 - Deploy Azure Firewall (30 minutes)

1.  In the Azure portal, select **+ Create a resource**. In the **Search the Marketplace** text box, type **Firewall**, in the list of results, select **Firewall**, and on the **Firewall** blade, select **Create**.

2.  On the **Create a firewall** blade, on the **Basics** tab, enter the following information: 

    -  Subscription: select your subscription.

    -  Resource group: **WGVNetRG1**

    -  Name: **AzureFirewall**

    -  Region: This must match the location in which you created the **WGVNet1** virtual network.

    -  Availability zone: **None**

    -  Firewall tier: **Standard**

    - Firewall management: **Use a Firewall Policy to manage this firewall**

    - Firewall policy: Click **Add new** and Policy name **AzureFirewall**

    -  Choose a Virtual network: **Use existing**

    -  Virtual network: **WGVNet1**

    -  Public IP address: **(Add new) AzureFirewall-PI**

3.  Select **Review + create** and then select **Create** to provision the Azure Firewall. 

1. Within 5-15 minutes, the resource group **WGVNetRG1** will have the firewall created. Next, we will firewall rules to allow the inbound and outbound traffic.

1.  On the main Azure menu select **Resource groups**.

2.  Select the **WGVNetRG1** resource group. This resource group contains the azure firewall and its public IP address resources.

3.  Navigate to the **AzureFirewall-PI** blade and note the value of its public IP address. You will need it later in this task.

4.  Navigate to the **AzureFirewall** blade, and, on the **Overview** page, select **Azure Firewall Policy** and **Rules** under **Settings** on the left.

5.  Select **+ Add NAT Rule collection** and enter the following information to create an inbound NAT Rule (collection is a list of rules that share the same priority and action) then select **Add**:

    -  Name: **NATRuleCollection1**

    -  Priority: **250**

    -  Rules name: **IncomingHTTP**

    -  Protocol: **TCP**

    -  Source: **\***

    -  Destination Address: Type the public IP address assigned to the firewall you identified earlier in this task.

    -  Destination ports: **80** (to allow HTTP traffic)

    -  Translated Address: **10.8.0.4** (Private IP of the WSWEB1 you deployed earlier in this lab.)
        
    -  Translated Port: **80**

7.  Select **Save** and wait until the update completes.

8.  Back on the Azure Firewall **Rules** page, select **Network rule collection**. Then Select **+ Add Network Rule collection** and enter the following information to create a Network Rule for inbound traffic. This rule allows HTTP connectivity from any directly connected network targeting the frontend IP address of the load balancer.

    -  Name: **NetworkRuleCollectionAllow1**

    -  Priority: **100**

    -  Action: **Allow**

    -  Rules name (IP Addresses): **IncomingWeb**

    -  Protocol: **TCP**

    -  Source: **\***

    -  Destination Address: **10.8.0.4**

    -  Destination ports: **80**

10. Select **Add** and wait until the update completes.

1.  In the Azure portal, navigate to the blade of the **WGVNetRG2** resource group.

2.  Select **AppRT**, followed by **Subnets** and then select **+ Associate**.

3.  On the **Associate subnet** blade, select **WGVNet2** on the **Virtual network** drop down. Select **AppSubnet** on the **Subnet** dropdown. 

4.  Select **OK** at the bottom of the **Associate subnet** blade.

5.  Navigate to the blade of the **WGVNetRG1** resource group, and select **MgmtRT**, then **Subnets**.

6.  Select **+ Associate**.

7.  On the **Associate subnet** blade, select **WGVNet1** on the **Virtual network** drop down. Select **Management** on the **Subnet** dropdown. 

8.  Select **OK** at the bottom of the **Associate subnet** blade.

1. Start Browser and navigate on **Public IP Address** of the **AzureFirewall**.

## Exercise 8 - Configure VPN Gateway Site-to-Site (30 minutes)

1.  In the Azure portal, select **+ Create a resource**, then in the **Search the Marketplace** box, search for and select **Virtual network**. Select **Create**.

2.  On the **Create virtual network** blade, enter the following information:

    -  Subscription: **Select your subscription**.

    -  Resource group: Select **Create new**, and enter the name **OnPremVNetRG**.

    -  Name: **OnPremVNet**

    -  Region: **(US) East US or available for you**.

3.  Leave the other options with their default values.

4.  Upon completion, it should look like the following screenshot. Validate the information is correct, and select **Next: IP Addresses**.

5. On the **IP addresses** tab of the **Create virtual network blade**, enter the following information.

    -  Address space: **192.168.0.0/16**

    - Select **+ Add subnet** then enter the following information in the blade that appears on the right and select **Add**.

      -  Subnet name: **default**

      -  Subnet address range: **192.168.0.0/24**

6. Select **Review + create** then **Create**.

1.  Select the **OnPremVnetRG** Resource Group and then open the **OnPremVNet** blade and select **Subnets**.

2.  Next, select **+ Gateway subnet**.

3.  Specify the following configuration for the subnet, and select **Save**:

    -  Subnet address range: **192.168.1.0/27**

    -  Route table: **None** (We will add this later.)

4.  Next, select **+ Subnet** and add the **OnPremManagementSubnet** subnet to the **OnPremVNet**, as shown below in the screenshot:

    - Name: **OnPremManagementSubnet**
  
    - Address range: **192.168.2.0/29**
  
    - Leave the rest of the values as their defaults. 

1.  Using the Azure Management portal, select **+ Create a resource**, type **Virtual Network gateway** in the **Search the Marketplace** text box, in the list of results, select **Virtual network gateway**, and then select **Create**.

2.  On the **Create virtual network gateway** blade,  enter the following information and select **Review + create**:

    -  Subscription: **Select your subscription**.

    -  Name: **OnPremWGGateway**

    -  Region: This must match the location in which you created the **OnPremVNet** virtual network.

    -  Gateway type: **VPN**

    -  VPN type: **Route-based**

    -  SKU: **VpnGw1**

    -  Virtual network: **OnPremVNet**

    -  Public IP address: **Create new**

    -  Public IP address name: **OnPremWGGateway-PI1**

    -  Enable active-active mode: **Enabled**

    -  Second Public IP address name: **OnPremWGGateway-PI2**

    -  Configure BGP: **Disabled**

3.  Validate your settings and select **Review + Create** then **Create**.

    >**Note:** The gateway will take 20-30 minutes to provision. Rather than waiting, continue to the next task.

1.  Using the Azure Management portal, select **+ Create a resource**, type **Virtual Network gateway** in the **Search the Marketplace** text box, in the list of results, select **Virtual network gateway**, and then select **Create**.

2.  On the **Create virtual network gateway** blade,  enter the following information and select **Review + create**:

    -  Subscription: **Select your subscription**.

    -  Name: **WGVNet1Gateway**

    -  Region: This must match the location in which you created the **WGVNet1** virtual network.

    -  Gateway type: **VPN**

    -  VPN type: **Route-based**

    -  SKU: **VpnGw1**

    -  Virtual network: **WGVNet1**

    -  Resource group: **WGVNetRG1**

    -  Public IP address: **Create new**

    -  Public IP address name: **WGVNet1Gateway-PI1**

    -  Enable active-active mode: **Enabled**

    -  Second Public IP address name: **WGVNet1Gateway-PI2**

    -  Configure BGP: **Disabled**

3.  Validate your settings and select **Review + Create** then **Create**.

    >**Note:** The gateway will take 30-45 minutes to provision. You will need to wait until both gateways are provisioned before proceeding to the next section.

4.  The Azure portal will display a notification when the deployments have completed.

1.  In the Azure portal, select **+ Create a resource**, in the **Search the Marketplace** text box, type in **Connection**, and press **Enter**.

2.  On the **Connection** blade, select **Create**.

3.  On the **Basics** blade, leave the **Connection type** set to **VNet-to-VNet**. Select the existing **WGVNetRG1** resource group. Enter the following information and select **Next: Settings**:
    - Establish bidirectional connectivity - checked
    - First connection name - **WGVNet1-to-OnPremWGGateway**
    - Second connection name - **WGGateway-to-WGVNet1**
    - Region - **East US US or available for you**

4.  On the Settings step, select **WGVNet1Gateway** as the first virtual network gateway and **OnPremWGGateway** as the second virtual network gateway. Ensure **Establish bidirectional connectivity** and **IKEV2** is selected. Enter a shared key, such as **A1B2C3D4**. Select **Review + create**.

5.  Select **Create** on the **Summary** page to create the connection.

6.  In the Azure portal, select **All services** on the left navigation. Then, type **connections** in the search text box and select **Connections**.

7.  Watch the progress of the connection status, and use the **Refresh** icon until the status changes for both connections from **Unknown** to **Connected**. This may take 5-10 minutes or more. You might need to refresh the page to see the change in status.

1. Create a new Virtual Machine for On-premises.

1.  On the Azure portal select **All services** at the left navigation. Enter **Route** in the search box, and select **Route tables**.

2.  On the **Route tables** blade, select **+ Add**.

3.  On the **Create route table** blade, enter the following information:

    -  Subscription: **Select your subscription**.

    -  Resource group: Select the drop-down menu, and select **WGVNetRG1**.

    -  Region: This must match the location in which you created the **WGVNet1** virtual network.

    -  Name: **WGAzureVNetGWRT**

    -  Propagate gateway routes: **Yes**

4.  Select **Review + create** then **Create**.

5.  Select the **WGAzureVNetGWRT** route table.

6.  Select **Routes** under **Settings** on the left.

7.  On the **Routes** blade, select the **+ Add** button. Enter the following information, and select **OK**:

    -  Route name: **OnPremToAppSubnet**

    -  Address prefix: **10.8.0.0/25**

    -  Next hop type: **Virtual appliance**

    -  Next hop address: **10.7.1.4**

8.  On the **WGAzureVNetGWRT - Routes** blade, select **Subnets** under **Settings** on the left.

9.  On the **Subnets** blade, select **+ Associate**.

10. On the **Associate subnet** blade, select **WGVNet1** under the **Virtual Network** drop down and select **GatewaySubnet** under the **Subnet** drop down.

    >**Note:** At this point, you have configured your enterprise network. You should be able to test your Enterprise Class Network from one region to another. Your testing can include the following scenarios:

    -  On the 'on-premises' virtual machine (OnPremVM), attempt to initiate a Remote Desktop session to any virtual machine on the AppSubnet (10.8.0.0/25). Note that this should fail since it is blocked by Azure Firewall.

    -  In the Azure portal, navigate to the WGWEB1 VM and initiate a Bastion connection session to the WGWEB1 virtual machine by selecting **Connect** and **Bastion**. This should be successful since it is allowed by Azure Firewall and Azure Bastion Host. 

    -  From within the WGWEB1 VM Bastion connection session, initiate a Remote Desktop session to the WGSQL1 via its private IP address (10.8.1.4). This should be successful since it is allowed by Azure Firewall.

## Project - Deploy Hub-spoke Archicture (30 minutes)

Implement a Hub-spoke topology

   ![Screenshot of the Hub-spoke](/AllFiles/Images/Hub-Spoke.png)

**Important Notes**
- Three Virtual Networks;
- Application on the Spoke 1 network
- VNET Peering connection Hub and Spokes;
- Gateway VPN on the Hub network and On-premises network;
- Azure Bastion on the Hub network;
- Azure Firewall allow all internal traffic.
- Custom Route tables to address prefix "Address Space networking destination" and next hop type to virtual applicance;
Network rule 
- Azure Firewall on the Hub network;

References: [Hub-spoke network topology](https://docs.microsoft.com/en-us/azure/architecture/reference-architectures/hybrid-networking/hub-spoke)

## After the Hands-on lab 

1. Delete all Azure resources created in support of this Hands-on lab.

1. End of **Workshop Day**

1. Continue in the **Mentoria Arquiteto Cloud**.