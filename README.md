# AWS-VPC-Peering-Project
Overview

This project demonstrates how to set up two VPCs (Virtual Private Clouds) – a Test VPC and a Production (Prod) VPC – and then establish VPC Peering between them. The steps include:

Creating a Test VPC

Configuring a Subnet and EC2 Instance

Setting up an Internet Gateway and Route Table

Repeating the process for Prod VPC

Establishing VPC Peering between the two VPCs

Testing the connection using Ping commands

This guide provides step-by-step instructions to ensure the correct setup.

![image](https://github.com/user-attachments/assets/e4d9746d-697c-4291-bb36-c66d2db27140)

Step 1: Creating the Test VPC

1.1 Create a Test VPC

Open the AWS Management Console

Navigate to VPC Dashboard → Create VPC

Enter the details:

VPC Name: Test VPC

IPv4 CIDR: 10.0.0.0/16

Click Create VPC

<img width="891" alt="Screenshot 2025-03-03 at 5 28 57 PM" src="https://github.com/user-attachments/assets/fbff044c-4598-4733-9afc-82c66e6600bb" />

1.2 Create a Subnet in Test VPC

Go to Subnets → Click Create Subnet

Select Test VPC

Enter the details:

Subnet Name: Test Subnet

CIDR Block: 10.0.1.0/24

Click Create Subnet

1.3 Launch an EC2 Instance

Navigate to EC2 Dashboard → Launch Instance

Select an AMI (e.g., Amazon Linux 2)

Choose the Test VPC and Test Subnet

Enable Auto-assign Public IP: No (this makes the instance private)

Click Launch and connect using SSH

Note: You will get a connection error because the instance is private. We need to attach an Internet Gateway to enable connectivity.

Step 2: Configuring Internet Gateway & Route Table

2.1 Attach an Internet Gateway

Go to VPC Dashboard → Internet Gateways

Click Create Internet Gateway

Name it Test-IGW and Click Create

Select Test-IGW, Click Attach to VPC, and choose Test VPC

2.2 Update the Route Table

Go to VPC Dashboard → Route Tables

Select the Route Table for Test VPC

Click Edit Routes → Add a new route:

Destination: 0.0.0.0/0

Target: Test-IGW

Click Save

Now, your EC2 instance in Test VPC should be accessible via SSH.

![image](https://github.com/user-attachments/assets/aaac927e-5a60-4049-b8cc-1b7ccc665bbe)

![image](https://github.com/user-attachments/assets/129939a1-4249-4e2e-ac3e-78994a415d84)

![image](https://github.com/user-attachments/assets/7b4bc953-9a1d-4eab-877f-0aa303549330)

![image](https://github.com/user-attachments/assets/9842098d-41d6-451a-b3be-45c94952c961)

Step 3: Setting Up the Prod VPC

Repeat Step 1 and Step 2 for the Prod VPC with the following values:

VPC Name: Prod VPC

IPv4 CIDR: 192.168.0.0/16

Subnet CIDR: 192.168.1.0/24

Internet Gateway: Prod-IGW

Once done, launch an EC2 instance in Prod VPC and ensure connectivity.

![image](https://github.com/user-attachments/assets/4744b489-cf7c-46c4-af32-f53c5611ddc6)

Step 4: Establishing VPC Peering

4.1 Create a VPC Peering Connection

Go to VPC Dashboard → Peering Connections

Click Create Peering Connection

Enter the details:

Peering Name: Test-Prod Peering

Requester VPC: Test VPC

Accepter VPC: Prod VPC

Click Create Peering Connection

![image](https://github.com/user-attachments/assets/428b9e19-3a5b-47be-8112-73cedc8b93dd)

4.2 Accept the Peering Request

Go to VPC Peering Connections

Select the pending request and Click Accept

4.3 Update Route Tables for Peering

Go to Route Tables → Select Test VPC route table

Click Edit Routes → Add a new route:

Destination: 192.168.0.0/16

Target: Peering Connection

Click Save

<img width="1440" alt="Screenshot 2025-03-03 at 7 10 36 PM" src="https://github.com/user-attachments/assets/c0aa64c7-0c34-4dcb-9c3a-9bc33de92537" />

Repeat the same steps for Prod VPC, adding a route for 10.0.0.0/16

Step 5: Testing VPC Peering Connection

Connect to the EC2 instance in Test VPC via SSH

![image](https://github.com/user-attachments/assets/e1178b9f-221e-491f-b6cd-0a58e3ea5d37)

![image](https://github.com/user-attachments/assets/142a6d8b-7a51-498b-9d4f-01ced206d59e)

Run the following command to test connectivity with Prod VPC:

ping <Prod VPC EC2 Private IP>

If the ping is successful, the VPC Peering setup is complete! 

Conclusion

By implementing this project, we successfully created two separate VPC environments and established VPC Peering to enable seamless communication between them. This setup is commonly used in real-world multi-tier architectures, where different VPCs are isolated for security but still require controlled access.

Through this hands-on exercise, we learned how to:
✅ Configure custom VPCs with subnets and routing
✅ Set up Internet Gateways and manage route tables
✅ Establish secure peering connections between VPCs
✅ Test network connectivity using simple ping commands

This project serves as a foundation for building more complex network architectures in AWS. Expanding on this, we can explore VPC endpoints, NAT gateways, and security group policies to enhance security and performance. 

