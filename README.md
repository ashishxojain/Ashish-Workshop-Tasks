# AWS VPC: Public - Private Architecture Setup

**Author**: Ashish Jain  
**Reg No**: 11721210004  
**Workshop Task Documentation**

---

## Task 1: Manual Setup via AWS Console

---

### 1. Create a VPC
- **Action**: Created a custom VPC with CIDR `10.0.0.0/16`
- **Option Enabled**: DNS hostnames  
![VPC Creation](screenshots/vpc1.png)

---

### 2. Create Subnets
- **Public Subnet**: `10.0.1.0/24`  
![VPC Creation](screenshots/publicSubnet.png)
- **Private Subnet**: `10.0.2.0/24`  
![VPC Creation](screenshots/privateSubnet.png)
- **Region/Zone**: e.g., `ap-south-1`  


---

### 3. Create and Attach Internet Gateway (IGW)
- **Step 1**: Created IGW  
![VPC Creation](screenshots/IGBefore.png)
- **Step 2**: Attached IGW to the VPC  
![VPC Creation](screenshots/IGAfter.png)


---

### 4. Create NAT Gateway
- **Step 1**: Allocated Elastic IP  
![VPC Creation](screenshots/ElasticIP.png)
- **Step 2**: Created NAT Gateway in Public Subnet using Elastic IP  
![VPC Creation](screenshots/NAT.png)
- **Step 3**: Waited until status became "Available"  
![VPC Creation](screenshots/NATFinal.png)


---

### 5. Create Route Tables

#### Public Route Table:
- **Step 1**: Created Public RT  
![VPC Creation](screenshots/PublicRT1.png)
- **Step 2**: Route added: `0.0.0.0/0 -> IGW`  
![VPC Creation](screenshots/PublicRT2.png)
- **Step 3**: Associated with Public Subnet  
![VPC Creation](screenshots/PublicRT3.png)

#### Private Route Table:
- **Step 1**: Created Private RT  
![VPC Creation](screenshots/PrivateRT1.png)
- **Step 2**: Route added: `0.0.0.0/0 -> NAT Gateway`  
![VPC Creation](screenshots/PrivateRT2.png)
- **Step 3**: Associated with Private Subnet  
![VPC Creation](screenshots/PrivateRT3.png)

---

### 6. Configure Security Groups

#### Bastion SG
- **Inbound**: SSH (port 22) from My IP  
- **Outbound**: Allow All  
![VPC Creation](screenshots/BastionSG.png)

#### Backend EC2 SG
- **Inbound**: SSH (port 22) from `10.0.1.0/24`  
- **Outbound**: Allow All  
![VPC Creation](screenshots/BackendSG.png)

---

### 7. Launch EC2 Instances

#### Bastion Host (Public Subnet)
- **AMI**: Ubunti
- **Key Pair**: `bastion.pem`  
- **Subnet**: Public  
- **SG**: BastionSG  
**📷 Screenshots**: 
![VPC Creation](screenshots/BastionEC21.png)
![VPC Creation](screenshots/BastionEC22.png)



#### Backend EC2 (Private Subnet)
- **AMI**: Amazon Linux 2 / Ubuntu  
- **Key Pair**: `Backend.pem`  
- **Subnet**: Private  
- **SG**: BackendSG  
**📷 Screenshot**: 
![VPC Creation](screenshots/BackendEC21.png)
![VPC Creation](screenshots/BackendEC22.png)

---

### 8. Validation Screenshots
- **Validate Bastion Access**
![VPC Creation](screenshots/BastionValidation.png)
- **Validate Backend Access**
![VPC Creation](screenshots/BackendValidation.png)
- **Validate Internet Access**
- *Ping google.com*
![VPC Creation](screenshots/GooglePing.png)
- *Updating Ubuntu using APT install*
![VPC Creation](screenshots/APTInstall.png)