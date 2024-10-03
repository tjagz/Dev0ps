



# AWS VPC PROJECT


* A Virtual Private Cloud (VPC) is a secure, isolated section of a cloud provider's infrastructure. It allows users to provision and manage a logically isolated network environment where they can deploy resources such as virtual machines, storage, and databases.

## Key Features of VPC:

### Isolation:

- VPCs are isolated from other virtual networks in the cloud, providing a secure environment for your resources.


### Customizable Network Configuration:

- You can define your IP address range, create subnets, and configure route tables and network gateways.


### Security:

- VPCs allow you to implement security measures such as firewalls, security groups, and network access control lists (ACLs) to control traffic flow.


### Connectivity:

- VPCs can be connected to your on-premises network through a VPN or dedicated connection, facilitating hybrid cloud architectures.


### Scalability:

You can easily scale resources up or down as needed, adapting to changing workloads.

## Use Cases for VPC:

- Hosting Web Applications: Create a secure environment for web applications while controlling inbound and outbound traffic.

- Data Storage: Store sensitive data securely with controlled access.
Development and Testing: Provide a separate environment for development and testing without affecting production systems.


### In summary, a VPC gives you control over your virtual networking environment, making it easier to manage resources securely in the cloud.




# DOCUMENTION


- The Following are the VPC details, region, and availability zones used for the project.

- CIDR Block: 10.0.0.0/16
- Region: us-west-2
- Availability Zones: us-west-2a, us-west-2b, us-west-2c
- Subnets: 15 Subnets (One per availability Zone)






##  Logged into my AWS management console and changed my region to us-west-2 (oregon)



![1](/img7/1.png)




## Searced for VPC in the search box



![2](/img7/2.png)




- Clicked on create VPC



![3](/img7/3.png)



- Selected VPC only, created a tag name, maintained the exisiting 10.0.0.0/24 IPV4 CIDR and clicked on create VPC



![4](/img7/4.png)







## Clicked on Internet gateway and create internet gateway


![5](/img7/5.png)



![6](/img7/6.png)




- Created internet gateway by creating a name tag


![7](/img7/7.png)





- I attached the internet gateway to the created VPC



![8](/img7/8.png)






![9](/img7/9.png)




## Created subnets for public, application, database, management and platform with the table below;

| Subnet Name              | Availability Zone | CIDR Block    | Type    |
|--------------------------|-------------------|---------------|---------|
| **Public**               |                   |               |         |
| Prod-Web-Public-2a       | us-west-2a        | 10.0.0.0/28   | Public  |
| Prod-Web-Public-2b       | us-west-2b        | 10.0.0.16/28  | Public  |
| Prod-Web-Public-2c       | us-west-2c        | 10.0.0.32/28  | Public  |
| **App Subnets**          |                   |               |         |
| Prod-App-Private-2a      | us-west-2a        | 10.0.0.48/28  | Private |
| Prod-App-Private-2b      | us-west-2b        | 10.0.0.64/28  | Private |
| Prod-App-Private-2c      | us-west-2c        | 10.0.0.80/28  | Private |
| **DB Subnets**           |                   |               |         |
| Prod-DB-Private-2a       | us-west-2a        | 10.0.0.96/28  | Private |
| Prod-DB-Private-2b       | us-west-2b        | 10.0.0.112/28 | Private |
| Prod-DB-Private-2c       | us-west-2c        | 10.0.0.128/28 | Private |
| **Management Subnets**   |                   |               |         |
| Prod-Mgmt-Private-2a     | us-west-2a        | 10.0.0.144/28 | Private |
| Prod-Mgmt-Private-2b     | us-west-2b        | 10.0.0.160/28 | Private |
| Prod-Mgmt-Private-2c     | us-west-2c        | 10.0.0.176/28 | Private |
| **Platform Subnets**     |                   |               |         |
| Prod-Platform-Private-2a | us-west-2a        | 10.0.0.192/28 | Private |
| Prod-Platform-Private-2b | us-west-2b        | 10.0.0.208/28 | Private |
| Prod-Platform-Private-2c | us-west-2c        | 10.0.0.224/28 | Private |
|                          |                   |               |         |




- Clicked on subnets and create subnet


![10](/img7/10.png)




![11](/img7/11.png)




![12](/img7/12.png)




![13](/img7/13.png)




![14](/img7/14.png)



- How it looked after creating for all the subnets


![15](/img7/15.png)



##  Created route table and assigned rules for each of the subnet group

- Assigned all three public subnets to same public-subnet route table


- Using the table below, I created 5 route tables;


| **Subnet** | **Destination CIDR** | **Target**       |
|------------|----------------------|------------------|
| Public     | 0.0.0.0/0            | Internet Gateway |
| App        | 0.0.0.0/0            | Nat Gateway      |
| DB         | 0.0.0.0/0            | Nat Gateway      |
| Management | 0.0.0.0/0            | Nat Gateway      |
| Platform   | 0.0.0.0/0            | Nat Gateway      |





- Clicked on route table and create route table



![16](/img7/16.png)





- Tagged a name and selected the VPC created 



![17](/img7/17.png)




- Associated the public subnet to the route table





![18](/img7/18.png)






- Added the 3 public subnets (prod-web-public a,b,c)




![19](/img7/19.png)



***Using the table and steps above, I created the other route tables and their subnet association***



### APPLICATION


![20](/img7/20.png)





### DATABASE


![21](/img7/21.png)




### MANAGEMENT


![22](/img7/22.png)



### PLATFORM


![23](/img7/23.png)







- Looked like this after creating all 5 



![24](/img7/24.png)





## Created NAT gateway and attached it to the route tables created 





![25](/img7/25.png)





- Selected a subnet and allocated an elastic ip





![26](/img7/26.png)





- Added the NAT gateway to the route tables one by one



### Started with Platform 


![27](/img7/27.png)




- Selected ***0.0.0.0/0*** for Destination

- Selected ***NAT gateway*** for target

- selected the NAT gateway created earlier and saved changes



![28](/img7/28.png)




- The status of the NAT gateway should be active





![29](/img7/29.png)





### Did for Database route table


![30](/img7/30.png)



- Did for other ***route tables ie for the  APP and Managment route tables also***




### For the public subnet, routed traffic using a service called internet gateway. 


- Attached internet gateway to the public route table


![31](/img7/31.png)




![32](/img7/32.png)





![33](/img7/33.png)



## AWS VPC Topology

***Note***: Both the internet Gateway (IGW) and NAT gateway(NAT-GW) gets deployed in the public subnet


- Checked the VPC topology


![34](/img7/34.png)






![35](/img7/35.png)



## Network ACLs


- Network access control list (NACL) is the native VPC functionality to control the inbound and outbound traffic at the subnet level.



- Used the below table as a guide to how my implemetation would look like: 


### DB NACL (Inbound Rules)

| Rule Number |     Type    | Protocol | Port Range |   Source IP   | Allow/Deny |
|:-----------:|:-----------:|:--------:|:----------:|:-------------:|:----------:|
| 100         | Custom TCP  | TCP      | 3306       | 10.0.0.96/28  | Allow      |
| 110         | Custom TCP  | TCP      | 3306       | 10.0.0.112/28 | Allow      |
| 120         | Custom TCP  | TCP      | 3306       | 10.0.0.128/28 | Allow      |
| *           | All Traffic | All      | All        | 0.0.0.0/0     | Deny       |





### DB NACL (Outbound Rules)


| Rule Number |     Type    | Protocol | Port Range | Destination IP | Allow/Deny |
|:-----------:|:-----------:|:--------:|:----------:|:--------------:|:----------:|
| 100         | Custom TCP  | TCP      | 3306       | 10.0.0.192/28  | Allow      |
| 110         | Custom TCP  | TCP      | 3306       | 10.0.0.208/28  | Allow      |
| 120         | Custom TCP  | TCP      | 3306       | 10.0.0.224/28  | Allow      |
| *           | All Traffic | All      | All        | 0.0.0.0/0      | Deny       |





- Here is a step by step on the Network ACLS:


- Clicked on Network ACL and create network ACL




![36](/img7/36.png)





![37](/img7/37.png)







![38](/img7/38.png)




- Updated the Inbound/Updated rules and saved changes



![39](/img7/39.png)



![41](/img7/41.png)



## THE FINAL LOOK


### INBOUND


![42](/img7/42.png)


### OUTBOUND


![43](/img7/43.png)




# THE END OF PROJECT























