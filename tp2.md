# I. Prérequis¶

## 2. Une paire de clés SSH¶

### A. Choix de l'algorithme de chiffrement¶

🌞 Déterminer quel algorithme de chiffrement utiliser pour vos clés

le chiffrement RSA apparaît particulièrement exposé face aux algorithmes quantiques. Les propriétés des qubits et des algorithmes quantiques rendent la factorisation beaucoup plus accessible qu’avec les machines classiques. (https://www.actualite-tech.fr/informatique-quantique-rsa/)


Représente le dernier cri en matière de cryptographie à courbes elliptiques. ED25519 offre une sécurité très élevée avec des clés de seulement 256 bits. Il est également intéressant de préciser qu’ED25519 est résistant à certaines attaques quantiques, contrairement à RSA, même si le vrai “quantum-proof” n’existe pas encore.(https://www.ethersys.fr/actualites/20241022-quelle-cle-ssh-generer/)

### B. Génération de votre paire de clés¶

🌞 Générer une paire de clés pour ce TP

```bash
azureuser@tp1:~$ ssh-keygen -t ed25519 -C tp1_cloud_user
Generating public/private ed25519 key pair.
Enter file in which to save the key (/home/azureuser/.ssh/id_ed25519):
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /home/azureuser/.ssh/id_ed25519
Your public key has been saved in /home/azureuser/.ssh/id_ed25519.pub
The key fingerprint is:
SHA256:atZ1v4ALoYg/uu2C35gICnGacl0MHtsjQQr/yXmmydY tp1_cloud_user
The key's randomart image is:
+--[ED25519 256]--+
|.   .            |
| o o             |
|  o +            |
|   + X           |
|. . O B S . .    |
| = + X = o o .   |
|*.o B E o . . .  |
|*o.B.o   . . . . |
|o.*==.    .   .  |
+----[SHA256]-----+
azureuser@tp1:~$ eval "$(ssh-agent -s)"
Agent pid 14736
azureuser@tp1:~$ ssh-add ~/.ssh/id_ed25519
Enter passphrase for /home/azureuser/.ssh/id_ed25519:
Identity added: /home/azureuser/.ssh/id_ed25519 (tp1_cloud_user)
```

🌞 Configurer un agent SSH sur votre poste



```bash
azureuser@tp1:~$ eval "$(ssh-agent -s)"
Agent pid 14736
azureuser@tp1:~$ ssh-add ~/.ssh/id_ed25519
Enter passphrase for /home/azureuser/.ssh/id_ed25519:
Identity added: /home/azureuser/.ssh/id_ed25519 (tp1_cloud_user)
azureuser@tp1:~$ eval "$(ssh-agent -s)"
Agent pid 14747
azureuser@tp1:~$ ssh-add ~/.ssh/id_ed25519
Enter passphrase for /home/azureuser/.ssh/id_ed25519:
Identity added: /home/azureuser/.ssh/id_ed25519 (tp1_cloud_user)
azureuser@tp1:~$ ssh-add -l
256 SHA256:atZ1v4ALoYg/uu2C35gICnGacl0MHtsjQQr/yXmmydY tp1_cloud_user (ED25519)
azureuser@tp1:~$ nano ~/.ssh/config
```

# II. Spawn des VMs¶

## 1. Depuis la WebUI¶

🌞 Connectez-vous en SSH à la VM pour preuve
```bash

Connection to 20.123.62.219 closed.
PS C:\Users\Asus> ssh azureuser@20.123.62.219
Welcome to Ubuntu 24.04.4 LTS (GNU/Linux 6.17.0-1008-azure x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/pro

 System information as of Mon Mar 30 15:16:11 UTC 2026

  System load:  0.0                Processes:             145
  Usage of /:   11.2% of 28.02GB   Users logged in:       1
  Memory usage: 7%                 IPv4 address for eth0: 10.0.0.4
  Swap usage:   0%

 * Strictly confined Kubernetes makes edge and IoT secure. Learn how MicroK8s
   just raised the bar for easy, resilient and secure K8s cluster deployment.

   https://ubuntu.com/engage/secure-kubernetes-at-the-edge

Expanded Security Maintenance for Applications is not enabled.

0 updates can be applied immediately.

1 additional security update can be applied with ESM Apps.
Learn more about enabling ESM Apps service at https://ubuntu.com/esm


*** System restart required ***
Last login: Mon Mar 30 15:13:32 2026 from 31.34.143.104
```
### 2. az : a programmatic approach¶

🌞 Créez une VM depuis le Azure CLI


```bash
azureuser@tp1:~$ az vm create -g tp_cloud_meo -n tp2-alma --size Standard_B1s --image almalinux:almalinux-x86_
64:10-gen2:10.1.202512150 --admin-username azureuser --
location denmarkeast --ssh-key-values "ssh-ed25519 AAAA
C3NzaC1lZDI1NTE5AAAAIBFrvl7OC+VXesszLab95+64AU8I8MzGjGV
Br31m77qS tp1_cloud_user"
The default value of '--size' will be changed to 'Standard_D2s_v5' from 'Standard_DS1_v2' in a future release.
Consider upgrading security for your workloads using Azure Trusted Launch VMs. To know more about Trusted Launch, please visit https://aka.ms/TrustedLaunch.
{
  "fqdns": "",
  "id": "/subscriptions/bb55c87a-02ce-4277-bc90-841c66aaf008/resourceGroups/tp_cloud_meo/providers/Microsoft.Compute/virtualMachines/tp2-alma",
  "location": "denmarkeast",
  "macAddress": "7C-ED-8D-6B-01-63",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "9.205.154.165",
  "resourceGroup": "tp_cloud_meo"
}
```
🌞 Assurez-vous que vous pouvez vous connecter à la VM en SSH sur son IP publique


```bash
azureuser@tp1:~$ ssh azureuser@9.205.154.165
The authenticity of host '9.205.154.165 (9.205.154.165)' can't be established.
ED25519 key fingerprint is SHA256:v+b66f3l/7mX6/3bzu0PPoueNqUTTzombJj+weWVPp4.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '9.205.154.165' (ED25519) to the list of known hosts.
[azureuser@tp2-alma ~]$
```

🌞 Une fois connecté, prouvez la présence...

```bash
[azureuser@tp2-alma ~]$ systemctl status waagent.service
● waagent.service - Azure Linux Agent
     Loaded: loaded (/usr/lib/systemd/system/waagent.s>
     Active: active (running) since Mon 2026-03-30 15:>
 Invocation: e2a853c95eab45208b0757b3769f1b7e
   Main PID: 1320 (python3)
      Tasks: 6 (limit: 5168)
     Memory: 47.4M (peak: 48.9M)
        CPU: 1.805s
     CGroup: /azure.slice/waagent.service
             ├─1320 /usr/bin/python3 -u /usr/sbin/waag>
             └─1452 /usr/bin/python3 -u bin/WALinuxAge>

[azureuser@tp2-alma ~]$ cloud-init status
status: done
[azureuser@tp2-alma ~]$ systemctl status cloud-init.service
● cloud-init.service - Cloud-init: Network Stage
     Loaded: loaded (/usr/lib/systemd/system/cloud-ini>
     Active: active (exited) since Mon 2026-03-30 15:4>
 Invocation: 1e175cb192ba4614ae6a414bf4bf6d2b
   Main PID: 941 (code=exited, status=0/SUCCESS)
   Mem peak: 49.7M
        CPU: 823ms


```
🌞 Utilisez Terraform pour créer une VM dans Azure

```bash
azureuser@tp1:~/terraform-deploy$ terraform init
Initializing the backend...
Initializing provider plugins...
- Reusing previous version of hashicorp/azurerm from the dependency lock file
- Using previously-installed hashicorp/azurerm v4.66.0

Terraform has been successfully initialized!

You may now begin working with Terraform. Try running "terraform plan" to see
any changes that are required for your infrastructure. All Terraform commands
should now work.

If you ever set or change modules or backend configuration for Terraform,
rerun this command to reinitialize your working directory. If you forget, other
commands will detect it and remind you to do so if necessary.
azureuser@tp1:~/terraform-deploy$ terraform apply

Terraform used the selected providers to generate the
following execution plan. Resource actions are
indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # azurerm_linux_virtual_machine.main will be created
  + resource "azurerm_linux_virtual_machine" "main" {
      + admin_username                                         = "azureuser"
      + allow_extension_operations                             = (known after apply)
      + bypass_platform_safety_checks_on_user_schedule_enabled = false
      + computer_name                                          = (known after apply)
      + disable_password_authentication                        = (known after apply)
      + disk_controller_type                                   = (known after apply)
      + extensions_time_budget                                 = "PT1H30M"
      + id                                                     = (known after apply)
      + location                                               = "denmarkeast"
      + max_bid_price                                          = -1
      + name                                                   = "super-vm"
      + network_interface_ids                                  = (known after apply)
      + os_managed_disk_id                                     = (known after apply)
      + patch_assessment_mode                                  = (known after apply)
      + patch_mode                                             = (known after apply)
      + platform_fault_domain                                  = -1
      + priority                                               = "Regular"
      + private_ip_address                                     = (known after apply)
      + private_ip_addresses                                   = (known after apply)
      + provision_vm_agent                                     = (known after apply)
      + public_ip_address                                      = (known after apply)
      + public_ip_addresses                                    = (known after apply)
      + resource_group_name                                    = "tp2_terraform"
      + size                                                   = "Standard_B1s"
      + virtual_machine_id                                     = (known after apply)
      + vm_agent_platform_updates_enabled                      = (known after apply)


```


📁 Fichiers à rendre

main.tf
```bash
# main.tf

provider "azurerm" {
  features {}
  subscription_id = var.subscription_id
}

resource "azurerm_resource_group" "main" {
  name     = var.resource_group_name
  location = var.location
}

resource "azurerm_virtual_network" "main" {
  name                = "vm-vnet"
  address_space       = ["10.0.0.0/16"]
  location            = azurerm_resource_group.main.location
  resource_group_name = azurerm_resource_group.main.name
}

resource "azurerm_subnet" "main" {
  name                 = "vm-subnet"
  resource_group_name  = azurerm_resource_group.main.name
  virtual_network_name = azurerm_virtual_network.main.name
  address_prefixes     = ["10.0.1.0/24"]
}

resource "azurerm_network_interface" "main" {
  name                = "vm-nic"
  location            = azurerm_resource_group.main.location
  resource_group_name = azurerm_resource_group.main.name

  ip_configuration {
    name                          = "internal"
    subnet_id                     = azurerm_subnet.main.id
    private_ip_address_allocation = "Dynamic"
    public_ip_address_id          = azurerm_public_ip.main.id
  }
}

resource "azurerm_public_ip" "main" {
  name                = "vm-ip"
  location            = azurerm_resource_group.main.location
  resource_group_name = azurerm_resource_group.main.name
  allocation_method   = "Static"
  sku                 = "Standard"
}

resource "azurerm_linux_virtual_machine" "main" {
  name                = "super-vm"
  resource_group_name = azurerm_resource_group.main.name
  location            = azurerm_resource_group.main.location
  size                = "Standard_B1s"
  admin_username      = var.admin_username
  network_interface_ids = [
    azurerm_network_interface.main.id,
  ]

  admin_ssh_key {
    username   = var.admin_username
    public_key = file(var.public_key_path)
  }

  os_disk {
    caching              = "ReadWrite"
    storage_account_type = "Standard_LRS"
    name                 = "vm-os-disk"
  }

  source_image_reference {
    publisher = "almalinux"
    offer     = "almalinux-x86_64"
    sku       = "10-gen2"
    version   = "latest"
  }
}

```
variable.tf

```bash
# variables.tf

variable "resource_group_name" {
  type        = string
  description = "Name of the resource group"
}

variable "location" {
  type        = string
  default     = "East US"
  description = "Azure region"
}

variable "admin_username" {
  type        = string
  description = "Admin username for the VM"
}

variable "public_key_path" {
  type        = string
  description = "Path to your SSH public key"
}

variable "subscription_id" {
  type        = string
  description = "Azure subscription ID"
}

```

terraform.tfvars

```bash
resource_group_name = "tp2_terraform"
admin_username = "azureuser"
public_key_path = "~/.ssh/id_ed25519.pub"
location = "denmarkeast"
subscription_id = "bb55c87a-02ce-4277-bc90-841c66aaf008"
```
🌞 Prouvez avec une connexion SSH sur l'IP publique que la VM est up



```bash
azureuser@tp1:~/terraform-deploy$ ssh azureuser@9.205.155.177
The authenticity of host '9.205.155.177 (9.205.155.177)' can't be established.
ED25519 key fingerprint is SHA256:cQ6AaWlxBXKELQ1uRtp4N8SrGmKd/EdSRnh4KsMDRRg.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '9.205.155.177' (ED25519) to the list of known hosts.
[azureuser@super-vm ~]$
```