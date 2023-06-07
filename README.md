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

![image](https://github.com/nkommareddy/AWS-Capstone-Project/assets/133917107/2b92765e-1c6c-4189-9a4f-cc26b8e11dca)
> *For the majority of this project, I had created my original instance with the instance type "t2.micro". However, after underestimating the applicant pool, I realized a more appropriate instance size would be "t3.medium". This change will not be reflected in a number of the remaining images in this documentation but it is a change I would've made earlier had I realized.*


I created an EC2 instance with the following settings. When creating the Amazon Machine Image (AMI), I chose to use Ubuntu as the platform type. This is primarily because of its compatibility with Bash. The user data being used in this project was coded in Bash so this decision seemed like a no brainer.  

I also created a security group that allowed traffic through the HTTP/S and SSH ports. This makes it so the user can actually access the website being hosted and so that the application could be load tested later on. 

> *I origninally included services like S3 and RDS to begin with. However, as I started to build out the infrastructure within the AWS platform, I began to realize a couple things. The decision to include or exclude specific AWS services like Amazon S3 and RDS depends on the specific requirements, scalability needs, and constraints of the web application at XYZ University.  If the web application does not require extensive file storage or the need for a dedicated object storage service, such as storing large media files, documents, or backups, then the use of Amazon S3 may not be necessary. Additionally, if the web application does not require a traditional relational database structure or complex queries, using Amazon RDS may not be essential. Therefore I decided that storing the data on an EC2 would be sufficient enough for the purposes of this project and decided not to include these surfaces.* 


#### Task 3: Testing the deployment

After my EC2 was configured, I was able to test whether or not the website application worked. After impatiently refreshing the IP address several times, I finally made it onto the landing page with minimal difficulty. 

![image](https://github.com/nkommareddy/AWS-Capstone-Project/assets/133917107/9b8ceac1-07da-479f-996f-dc82aaee3684)

From here I was able to perform the appropriate tasks such as viewing, adding, deleting, and modifying the student records.


### Phase 3: Implementing High Availability and Scalability

From here, I had to make sure that my application was highly available and scaleable. The two main features of AWS I used to implement this were Auto Scaling and the Application Load Balancer.  

#### Task 1: Creating an Application Load Balancer and Configuring Auto Scaling

This next step was quite straight forward. I set up my load balancer with the following settings: 
![image](https://github.com/nkommareddy/AWS-Capstone-Project/assets/133917107/bb9ba0fa-b196-47bf-a747-c696f2ee334a)

When setting up Auto Scaling, I created a tag (as shown below) signifying that a new instance had been created. I also set my auto scale settings to have a minimum of 2 scaled instances running with a maximum of 6 running at any given time. 

![image](https://github.com/nkommareddy/AWS-Capstone-Project/assets/133917107/dac5f673-e6ac-417f-a3fb-da4cd057af19) ![image](https://github.com/nkommareddy/AWS-Capstone-Project/assets/133917107/f90d2dc5-a31e-42e1-8f59-390753f89888)

#### Task 2: LOAD TESTING!!

This was it! The moment of truth! To load test, I used a method that utilized Cloud 9, AWS' IDE for writing, running, and debugging code. Within Cloud9, I inputted some code that sent an ungodly amount of requests to the load balancer I had created earlier. All I had to do now was wait and hope that more instances would be created. 

![image](https://github.com/nkommareddy/AWS-Capstone-Project/assets/133917107/7b0bfeec-7907-4fd9-8058-6bc972043c2d)

These were the instances I started with. 

![image](https://github.com/nkommareddy/AWS-Capstone-Project/assets/133917107/d3b5bb12-17b9-40da-838d-8ceae8f9bb3b)

After a few minutes, one of my alarms in CloudWatch triggered indicating that the CPU utilization had exceeded its capacity. 

![image](https://github.com/nkommareddy/AWS-Capstone-Project/assets/133917107/6e47892d-3c17-4e68-b896-fb94e0961cd2)
A few minutes after that, another instance had been created! I could've left the script in Cloud9 keep running, but this was enough to prove that the load balancing and auto scaling worked. 

## Final Architecture
![image](https://github.com/nkommareddy/AWS-Capstone-Project/assets/133917107/6a8d6873-133a-4446-ad1e-0696084bc74b)

> *Ultimately I did not end up needing services like RDS, WAF, and S3. However, I hope to come back to this project in the future and figure out ways to successfully implement those services as well.*

But until then, I think it's safe to say this project was a success!



