# TP3B Part1 - Create the base VM¶

## 2. Feu patate¶

🌞 Créez une VM azure (une commande az)

```bash 
azureuser@tp1:~$ az vm create -g tp3-template-rg -n tp3-vm --size Standard_B1s --image almalinux:almalinux-x86_64:10-gen2:10.1.202512150 --admin-username azureuser --ssh-key-values "ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIBFrvl7OC+VXesszLab95+
64AU8I8MzGjGVBr31m77qS tp1_cloud_user"
The default value of '--size' will be changed to 'Standard_D2s_v5' from 'Standard_DS1_v2' in a future release.
Consider upgrading security for your workloads using Azure Trusted Launch VMs. To know more about Trusted Launch, please visit https://aka.ms/TrustedLaunch.
{
  "fqdns": "",
  "id": "/subscriptions/bb55c87a-02ce-4277-bc90-841c66aaf008/resourceGroups/tp3-template-rg/providers/Microsoft.Compute/virtualMachines/tp3-vm",
  "location": "denmarkeast",
  "macAddress": "7C-ED-8D-37-C2-53",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "9.205.153.207",
  "resourceGroup": "tp3-template-rg"
}
```

## Part2 - Prepare the VM

### 1. Poser notre conf custom¶

🌞 Effectuez la conf suivante :

```bash
[azureuser@tp3-vm ~]$ sudo dnf update
AlmaLinux 10 - AppStream                                                                4.0 MB/s | 2.3 MB     00:00
AlmaLinux 10 - BaseOS                                                                    21 MB/s |  18 MB     00:00
AlmaLinux 10 - CRB                                                                      1.5 MB/s | 525 kB     00:00
AlmaLinux 10 - Extras                                                                    35 kB/s |  12 kB     00:00
Dependencies resolved.

[azureuser@tp3-vm ~]$ sudo dnf install -y htop vim bind-utils
Extra Packages for Enterprise Linux 10 - x86_64                                         5.5 MB/s | 5.7 MB     00:01
Last metadata expiration check: 0:00:02 ago on Wed 01 Apr 2026 05:48:58 PM UTC.
Dependencies resolved.

```

🌞 Proposer une suite de commandes

```bash
[azureuser@tp3-vm ~]$ sudo cloud-init clean --logs
[azureuser@tp3-vm ~]$ sudo rm -rf /var/lib/cloud/*
[azureuser@tp3-vm ~]$ sudo systemctl enable cloud-init
[azureuser@tp3-vm ~]$ sudo find /var/log -type f -exec truncate -s 0 {} +
[azureuser@tp3-vm ~]$ history -c
[azureuser@tp3-vm ~]$ history -w
[azureuser@tp3-vm ~]$ sudo rm -rf /var/lib/waagent/*
[azureuser@tp3-vm ~]$ sudo waagent -deprovision+user -force
WARNING! The waagent service will be stopped.
WARNING! All SSH host key pairs will be deleted.
WARNING! Cached DHCP leases will be deleted.
WARNING! root password will be disabled. You will not be able to login as root.
WARNING! /etc/resolv.conf will be deleted.
WARNING! azureuser account and entire home directory will be deleted.
2026-04-01T18:13:03.160463Z INFO MainThread Examine /proc/net/route for primary interface
2026-04-01T18:13:03.161068Z INFO MainThread Primary interface is [eth0]

```

## Part3 - Create a template
### 1. Créer le template¶


🌞 Let's go, balancez :
```bash
azureuser@tp1:~$ az vm deallocate --resource-group tp3-template-rg --name tp3-vm
azureuser@tp1:~$ az vm generalize --resource-group tp3-template-rg --name tp3-vm
azureuser@tp1:~$ az image create \
  --resource-group tp3-template-rg \
  --name alma_chad \
  --source tp3-vm \
  --hyper-v-generation V2
{
  "hyperVGeneration": "V2",
  "id": "/subscriptions/bb55c87a-02ce-4277-bc90-841c66aaf008/resourceGroups/tp3-template-rg/providers/Microsoft.Compute/images/alma_chad",
  "location": "denmarkeast",
  "name": "alma_chad",
  "provisioningState": "Succeeded",
  "resourceGroup": "tp3-template-rg",
  "sourceVirtualMachine": {
    "id": "/subscriptions/bb55c87a-02ce-4277-bc90-841c66aaf008/resourceGroups/tp3-template-rg/providers/Microsoft.Compute/virtualMachines/tp3-vm",
    "resourceGroup": "tp3-template-rg"
  },
  "storageProfile": {
    "dataDisks": [],
    "osDisk": {
      "caching": "ReadWrite",
      "diskSizeGB": 30,
      "managedDisk": {
        "id": "/subscriptions/bb55c87a-02ce-4277-bc90-841c66aaf008/resourceGroups/tp3-template-rg/providers/Microsoft.Compute/disks/tp3-vm_OsDisk_1_bbaf4990484b4d9988ac1357f1ccabd6",
        "resourceGroup": "tp3-template-rg"
      },
      "osState": "Generalized",
      "osType": "Linux",
      "storageAccountType": "Premium_LRS"
    }
  },
  "tags": {},
  "type": "Microsoft.Compute/images"
}
```

### 2. Tester le template¶

🌞 Lancer une VM à partir de votre template
```bash
azureuser@tp1:~$ az vm create \
  --resource-group tp3-template-rg \
  --name tp3-vm-template \
  --size Standard_B1s \
  --image alma_chad \
  --admin-username azureuser \
  --ssh-key-values /tmp/ssh/id_ed25519.pub \
>

The default value of '--size' will be changed to 'Standard_D2s_v5' from 'Standard_DS1_v2' in a future release.
{
  "fqdns": "",
  "id": "/subscriptions/bb55c87a-02ce-4277-bc90-841c66aaf008/resourceGroups/tp3-template-rg/providers/Microsoft.Compute/virtualMachines/tp3-vm-template",
  "location": "denmarkeast",
  "macAddress": "7C-ED-8D-37-C2-09",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.5",
  "publicIpAddress": "9.205.153.4",
  "resourceGroup": "tp3-template-rg"
}

```

🌞 Vérification !
```bash
azureuser@tp1:~$ ssh azureuser@9.205.153.4
The authenticity of host '9.205.153.4 (9.205.153.4)' can't be established.
ED25519 key fingerprint is SHA256:3NSC6MM9SL1LwgMJKeGl4cIj8cYrx6bJ++CsNpF96H8.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '9.205.153.4' (ED25519) to the list of known hosts.
Enter passphrase for key '/home/azureuser/.ssh/id_ed25519':
[azureuser@tp3-vm-template ~]$ sudo cloud-init status
status: done
[azureuser@tp3-vm-template ~]$ sudo systemctl status waagent
● waagent.service - Azure Linux Agent
     Loaded: loaded (/usr/lib/systemd/system/waagent.service; enabled; preset: enabled)
     Active: active (running) since Wed 2026-04-01 19:15:28 UTC; 2min 53s ago
 Invocation: 60751e412db047a68a405243d9da9159
   Main PID: 1321 (python3)
      Tasks: 6 (limit: 5158)
     Memory: 48M (peak: 50M)
        CPU: 1.516s
     CGroup: /azure.slice/waagent.service
             ├─1321 /usr/bin/python3 -u /usr/sbin/waagent -daemon
             └─1453 /usr/bin/python3 -u bin/WALinuxAgent-2.15.0.1-py3.12.egg -run-exthandlers

Apr 01 19:15:33 tp3-vm-template python3[1453]: 1: lo    inet6 ::1/128 scope host noprefixroute \       valid_>
Apr 01 19:15:33 tp3-vm-template python3[1453]: 2: eth0    inet6 fe80::7eed:8dff:fe37:c209/64 scope link nopre>
Apr 01 19:15:33 tp3-vm-template python3[1453]: 2026-04-01T19:15:33.672394Z WARNING EnvHandler ExtHandler Dhcp>
Apr 01 19:15:33 tp3-vm-template python3[1453]: 2026-04-01T19:15:33.675678Z INFO EnvHandler ExtHandler Using i>
Apr 01 19:15:33 tp3-vm-template python3[1453]: 2026-04-01T19:15:33.707102Z INFO ExtHandler ExtHandler
Apr 01 19:15:33 tp3-vm-template python3[1453]: 2026-04-01T19:15:33.707191Z INFO ExtHandler ExtHandler Process>
Apr 01 19:15:33 tp3-vm-template python3[1453]: 2026-04-01T19:15:33.707723Z INFO ExtHandler ExtHandler No exte>
Apr 01 19:15:33 tp3-vm-template python3[1453]: 2026-04-01T19:15:33.708298Z INFO ExtHandler ExtHandler Process>
Apr 01 19:15:33 tp3-vm-template python3[1453]: 2026-04-01T19:15:33.741407Z INFO ExtHandler ExtHandler Looking>
Apr 01 19:15:33 tp3-vm-template python3[1453]: 2026-04-01T19:15:33.747191Z INFO ExtHandler ExtHandler [HEARTB>
lines 1-22/22 (END)

```
## Part4 - Hardened template
