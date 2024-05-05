# Solution/Approach

> So the overall task was to create a cloud environment for a web-application with 3 vms(servers) for their dedicated work.

*To complete the assessment task I performed this steps*

## Steps:
_Step1_ : Setup the V-Net
![image](https://github.com/Aisu2635/Claw-Assessment/assets/104310687/49c77206-d8e7-491e-941c-1f032b8d9aaf)
- irrespective of the Task Order of first Creating VMs, I First Created the resource Group.
![image](https://github.com/Aisu2635/Claw-Assessment/assets/104310687/0a803aeb-c5ee-472d-8403-1860060fa910)
- Inside The resource Group I searched for Virtual Network And Created a V-net named `claw-net` with the ip range: `192.168.0.0/16`
- Inside the V-net I created 3 subnets each for the VMs and Assigned the respective Ip Ranges to them.

_Step2_ : Setup NSG  
![image](https://github.com/Aisu2635/Claw-Assessment/assets/104310687/3b70d125-009d-4967-9b8d-bc2310e5914d)
- created the Respective NSGs for all 3 subnets.
- for Frontend:
```JSON
{
    "securityRules": [
        {
            "name": "httpreqallow",
            "protocol": "TCP",
            "destinationPortRange": "80",
            "access": "Allow",
            "priority": 101,
            "direction": "Inbound"
        },
        {
            "name": "httpsreqallow",
            "protocol": "TCP",
            "destinationPortRange": "443",
            "access": "Allow",
            "priority": 100,
            "direction": "Inbound"
        },
        {
            "name": "AllowCidrBlockHTTPOutbound",
            "protocol": "TCP",
            "destinationPortRange": "80",
            "sourceAddressPrefix": "191.168.1.0/24",
            "access": "Allow",
            "priority": 111,
            "direction": "Outbound"
        },
        {
            "name": "AllowCidrBlockHTTPSOutbound",
            "protocol": "TCP",
            "destinationPortRange": "443",
            "sourceAddressPrefix": "192.168.1.0/24",
            "access": "Allow",
            "priority": 121,
            "direction": "Outbound"
        },
        {
            "name": "DenyAnyCustomAnyOutbound",
            "protocol": "*",
            "access": "Deny",
            "priority": 131,
            "direction": "Outbound"
        }
    ]
}

```
- For backend:
```JSON
{
    "securityRules": [
        {
            "name": "AllowCidrBlockHTTPSInbound",
            "protocol": "TCP",
            "destinationPortRange": "443",
            "sourceAddressPrefix": "192.168.1.0/24",
            "access": "Allow",
            "priority": 100,
            "direction": "Inbound"
        },
        {
            "name": "AllowCidrBlockHTTPInbound",
            "protocol": "TCP",
            "destinationPortRange": "80",
            "sourceAddressPrefix": "192.168.1.0/24",
            "access": "Allow",
            "priority": 110,
            "direction": "Inbound"
        },
        {
            "name": "AllowAnyHTTPSOutbound",
            "protocol": "TCP",
            "destinationPortRange": "443",
            "access": "Allow",
            "priority": 120,
            "direction": "Outbound"
        },
        {
            "name": "AllowAnyHTTPOutbound",
            "protocol": "TCP",
            "destinationPortRange": "80",
            "access": "Allow",
            "priority": 130,
            "direction": "Outbound"
        },
        {
            "name": "DenyAnyCustomAnyOutbound",
            "protocol": "*",
            "access": "Deny",
            "priority": 140,
            "direction": "Outbound"
        }
    ]
}
```

- For DB:
```JSON
{
    "securityRules": [
        {
            "name": "AllowCidrBlockMySQLInbound",
            "protocol": "TCP",
            "destinationPortRange": "3306",
            "sourceAddressPrefix": "192.168.2.0/24",
            "access": "Allow",
            "priority": 100,
            "direction": "Inbound"
        },
        {
            "name": "AllowAnyMySQLOutbound",
            "protocol": "TCP",
            "destinationPortRange": "3306",
            "destinationAddressPrefix": "192.168.2.0/24",
            "access": "Allow",
            "priority": 110,
            "direction": "Outbound"
        },
        {
            "name": "DenyAnyCustomAnyOutbound",
            "protocol": "*",
            "access": "Deny",
            "priority": 120,
            "direction": "Outbound"
        }
    ]
}
```

_Step 3_ : Network Watcher and NSG Flow logs
- with the created V-net I naviagted to the Network watcher in azure.  
![image](https://github.com/Aisu2635/Claw-Assessment/assets/104310687/9b951ff3-40a8-4e3b-9248-8be2ee8c9dee)
- Selected the Network and Configured the required NSGs for logging data in the created storage class.  
![image](https://github.com/Aisu2635/Claw-Assessment/assets/104310687/a548f656-bc30-4404-848a-4c1f89a525ec)

_Step 4_ : VM deployment
- Deployed 3 VMs inside the resource group with their respected subnets assigned while creating the VM.
- for deployment I selected Ubuntu VM for easy and cheap VM which will help save cost instead of deploying a windows licensed VM.
- While creating The frontend VM i assigned the Public IP so that it can be mapped and in future DNS address can be assigned easily.  
![image](https://github.com/Aisu2635/Claw-Assessment/assets/104310687/26151d14-323f-4a96-942e-3c845e7cb576)

_Step 5_ : VM configuratrion.
- Frontend VM needs to sun services like nodejs or asp.net so using SSH Login and key pair installed the ngnix proxy server on the Linux instance
- Backend VM needs to run RESTFUL API so using SSH installed  Apache, Node.js & Python.
- DB VM needs to run MYSQL databse so using SSH I Installed MySQL and Xampp
- And Setuped the Azure RI (better than Pay2Go service for predictive workload on the application.

## Problems Faced.
> The major Problem I faced was for VMs. I am using my own Azure Student account which only allows 4 vCPUs in single region. As I already had my personal Cloud Hosted Kali VM 2 out of 4 vCPUs were occupied so I had to Shift Machines to different region  
> No project Data, Since no data was given about the frontend,backend application I couldnt Decide the appropriate OS,Appropriate Disk & Appropriate Machine to host the VM  
> AWS VPC vs Azure V-Net: I have experience of AWS VPC configuration and I found AWS more easier and manageable than Azure.







