---
layout: post
title:  "AzureWireGuard - An Azure ARM template to quickly setup your own WireGuard VPN Server."
date:   2018-12-23 12:00:00 +0530
categories: [azure, wireguard]
tags: [azure, wireguard]
---

<b>AzureWireGuard</b> - The quickest way to setup your own modern <b>VPN</b> server. This is an <b>Azure ARM</b> template that you can quickly deploy to setup your own <b>WireGuard VPN Server</b> with auto-configured server and client configurations. All server configuration steps are taken care of and five ready to use client configuration files are generated. You just have to download them and configure your <b>WireGuard</b> clients to use them.

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fvijayshinva%2FAzureWireGuard%2Fmaster%2FAzureWireGuard%2FLinuxVirtualMachine.json" target="_blank">
    <img src="http://azuredeploy.net/deploybutton.png"/>
</a>
<a href="http://armviz.io/#/?load=https%3A%2F%2Fraw.githubusercontent.com%2Fvijayshinva%2FAzureWireGuard%2Fmaster%2FAzureWireGuard%2FLinuxVirtualMachine.json" target="_blank">
    <img src="http://armviz.io/visualizebutton.png"/>
</a>

<b>[WireGuard][wireguard] VPN</b> is a rethink of how VPN software are designed and is receiving genuine appreciation from the community. WireGuard works by adding a network interface wg0 (similar to eth0 or wlan0), and any network traffic that is routed to this interface gets securely encapsulated and sent over UDP to its peer. The packets between these peers is encrypted using public-key(asymmetric) cryptography. The peers have to generate and share the public keys with one another and use them while setting up the network interface.

This <b>Azure ARM</b> template simplifies the setup process by scripting all the required steps; it does the following 
- Create an <b>[Ubuntu Server][ubuntu] Virtual Machine</b>.
    - The only inputs you provide are the administrator username and password.
    - The name of all resources are generated automatically to avoid any conflicts.
- Install <b>WireGuard Server</b>.
- Configure <b>WireGuard Server</b>
    - Create Private and Public Keys for Server and Client.
    - Create the Server Configuration.
    - The <b>WireGuard</b> interface IP address is set to 10.13.13.1.
- Setup <b>NAT</b> on the server to forward client traffic to the internet.
- Start the <b>WireGuard</b> Interface.
- Configure <b>WireGuard</b> to auto start even after a reboot.
- Generate five client configuration files, which you can download and start using. 
    - The five clients are given the IP addresses 10.13.13.101, 10.13.13.102, 10.13.13.103, 10.13.13.104 and 10.13.13.105.
    - The Client <b>DNS</b> server is set to [1.1.1.1][dns].
- Enable <b>[UFW][ufw]</b> firewall.
- Install <b>Ubuntu Server</b> Upgrades.
- Schedule a reboot after 24 hours, to ensure all <b>Ubuntu Server</b> Upgrades are applied.

You can deploy this <b>Azure ARM</b> template with one of the following methods. Some knowledge of how [Azure ARM templates][azure-arm] work is really helpful.
#### Method 1 - From [Visual Studio][vs]
- Clone the [git repository][git-repo].
- Open the solution file in <b>Visual Studio</b> and deploy from <b>Visual Studio</b>.

#### Method 2 - From [Azure Deploy][azure-deploy]
- Hit the <b>[Deploy to Azure][azure-deploy-awg]</b> button. 

#### Other Methods
- There are multiple ways to deploy an <b>Azure ARM</b> template like  [Powershell][azure-ps], [Azure CLI][azure-cli], [Azure Portal][azure-portal] and [REST API][azure-rest].

This <b>Azure ARM</b> template creates five ready to use <b>WireGuard</b> client configuration files. The client configuration files are named wg0-client-one.conf, wg0-client-two.conf, wg0-client-three.conf, wg0-client-four.conf and wg0-client-five.conf and are located in the administrator user's home folder (~/).

You can use tools like <b>scp</b> or <b>pscp</b> to download the client configuration files directly from the server.
    
  scp &lt;admin-user&gt;@&lt;server-fqdn&gt;:/home/&lt;admin-user&gt;/wg0-client-one.conf /local/dir/
    
  pscp &lt;admin-user&gt;@&lt;server-fqdn&gt;:/home/&lt;admin-user&gt;/wg0-client-one.conf c:\local\

  Example: scp vmadmin@awgyj5lzwixbj3ng.westus.cloudapp.azure.com:/home/vmadmin/wg0-client-one.conf /local/dir/

If you are going to use these client configuration files on <b>Windows</b> clients, just remember these configuration files have <b>Linux</b> Line Endings <b>(LF)</b> while Windows WireGuard clients would expect <b>DOS</b> Line Endings <b>(CRLF)</b>.

The <b>AzureWireGuard</b> <b>ARM</b> template is available on <b>[GitHub][azurewireguard]</b>. Feel free to contribute.

[azure-arm]: https://docs.microsoft.com/en-us/azure/azure-resource-manager/
[wireguard]: https://www.wireguard.com/
[dns]: https://1.1.1.1/
[ubuntu]: https://www.ubuntu.com/server
[azure-portal]: https://portal.azure.com
[vs]: https://visualstudio.microsoft.com/vs/community/
[git-repo]: https://github.com/vijayshinva/AzureWireGuard
[azure-ps]: https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-template-deploy
[azure-cli]: https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-template-deploy-cli
[azure-rest]: https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-template-deploy-rest
[azure-deploy]: azuredeploy.net
[azure-portal]: https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-template-deploy-portal
[azure-deploy-awg]: https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fvijayshinva%2FAzureWireGuard%2Fmaster%2FAzureWireGuard%2FLinuxVirtualMachine.json
[azure-rg]: https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview#resource-groups
[ufw]: https://help.ubuntu.com/community/UFW
[azurewireguard]: https://github.com/vijayshinva/AzureWireGuard