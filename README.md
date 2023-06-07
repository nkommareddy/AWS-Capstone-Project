# AWS-Capstone-Project

## Scenario 
XYZ University, a small, selective private university with a student population below 10,000, faces challenges due to high application volumes exceeding processing capacities. Consequently, the university's website application experiences suboptimal performance and sluggishness. Given the sensitive nature of storing applicants' private information, ensuring robust security measures is something extremely important. 



## Solution 
Given the scenario, I was tasked with designing and constructing a comprehensive architecture for hosting a new web application in the AWS Cloud for XYZ University. The primary objectives are to ensure seamless support for all applicants, while guaranteeing high availability, scalability, load balancing, security, and optimal performance. This transition to cloud-based hosting will offer XYZ University a significantly more reliable alternative compared to their current data center hosting approach.

### Phase One: Planning the design and estimating the cost

![download (9)](https://github.com/nkommareddy/AWS-Capstone-Project/assets/133917107/54a8a92f-5b72-4aaf-a88c-3d792b76b0fc)
<img width="882" alt="pricing estimate" src="https://github.com/nkommareddy/AWS-Capstone-Project/assets/133917107/8251959d-808f-4f05-9b9c-6d5887fe194f">

The following images are what I imagined my architecture and 12 month cost projection to look like initially. I quickly realized that the original architecture I designed included many unnecessary features and services. The changes I made along the way as well as the reasoning as to why I made those changes will be reflected in *italics* here on out. Additionally, a final architectural diagram showing the final version of what my solution came to look like will be included at the end of the documentation. 

### Phase 2: Creating a basic functional web application
In this phase, I will start building the solution. My objective in this phase is to develop a functional web application that works on a single virtual machine within a virtual network that I will create.

#### Task 1: Creating a Virtual Network
The first thing I did was create a Virtual Private Network (VPC) for XYZ University. By creating a VPC, XYZ University can establish a private and isolated network environment within the AWS Cloud. This separation provides an additional layer of security, effectively shielding the university's resources from the public internet .By utilizing a VPC, XYZ University gains the ability to scale its resources and accommodate future growth easily. It can provision and manage a wide range of AWS resources, including EC2 instances, load balancers, and auto-scaling groups, within the VPC. This scalability ensures that the university's network infrastructure can adapt to the changing needs and demands of its applications and user base.


![image](https://github.com/nkommareddy/AWS-Capstone-Project/assets/133917107/eff17674-504c-42e1-81fc-3e41df46a92c)

> *I determined I could eliminate the need for NAT Gateways and Private Subnets like I had initially anticipated using. By eliminating private subnets and the need for NAT gateways, ultimately making everything public facing, the network configuration becomes simpler, reducing complexity in the architecture. Doing this would also result in cost optimization as there are no additional expenses for NAT gateway usage.*

#### Task 2: Creating a Virtual Machine




