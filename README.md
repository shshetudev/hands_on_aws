
## Flow at a glance

1. Create VPC
2. Define CIDR Block for VPC
3. Create Security Groups: Public, Private
4. Create Internet Gateway for the VPC -> Attach the Gateway to the VPC
5. Create Route table for each of the subnets: Public, Private -> Associate the route tables to different subnets -> Route out the public route table to the internet gateway
6. Connet to the public EC2 to the AWS console
7. Create a new private EC2 on private subnet -> Create a new private security group and attach the keypair
8. Run from local machine where it contains the (key-pair)pem file and copy the .pem file in my public ec2: `sudo scip -i <LOCAL_PEM_FILE> <LOCAL_PEM_FILE> ec2-user@<PUBLIC_EC2_IP>:/home/ec2-user`
9. We connect to the public ec2 and can see the `<PEM_FILE>`. 
10. As route tables by default support subnet to subnet communication, that's why we go inside the public EC2 -> Connect to private EC2 using the `<PRIVATE_EC2_IP>` from there
11. We write run this in public EC2 command line: `sudo ssh -i <PEM_FILE> ec2-user@<PRIVATE_EC2_IP>` -> We go inside private EC2 and run `sudo yum update -y` and we will face an error
12. We go to VPC/NAT Gateways -> Create a NAT gateway with public subnet
13. We go to the Private subnet's route table -> Edit route -> Add route -> Provide "0.0.0.0" -> Target: NAT Gateway -> Save
14. Now we connect to public EC2 -> Connect to private EC2 -> Run `sudo yum update -y` -> But we can not connect to the private subnet using ssh

### What is CIDR block?

- 10.0.0.0:
  - Here each of this digits are octats and the value of each of them is 8. so, 8x4=32 bits
  - /24 (last number) = 10.0.0.1 - 10.0.0.254
  - /16 (last two numbers) = 10.0.0.1 - 10.0.255.254
  - /8 (last three numbers) = 10.0.0.1 - 10.255.255.254
- We can calculate number of subnets possible from here: `https://www.calculator.net/ip-subnet-calculator.html`

### What is Security Group?

- It acts as a virtual firewall for EC2 instances to controll incoming and outgoing traffic
- Security group is related to EC2 instances and here we set rule to allow incoming traffic

### What is Gateway?

- In general, Gateway connects our VPC to another network.
- There are multiple types of Gateways:
  - Internet Gateway:
    - It is the features of VPC
    - It allows our subnets to the internet
    - We can have only one gateway per VPC
    - After creating a Gateway we need to attach it to the VPC
  - Transit Gateway
  - NAT Gateway

### What is NAT Gateway?

- NAT: Network Access Translation
- We can use it so that, instance of a private subnet can connect to serivces outside the vpc, but external services can not connect to this instance.
- We need to create the NAT gateway in public subnet.
- So, we can connect our private subnet route table to the NAT gateway, so that services in the private subnet can connect to the services outside vpc.
- So, our private EC2 instance using private subnet's route table, can reach out to the NAT gateway in the public subnet and use the internet.

### What is Route table?

- We need to allow our route table on our public subnet, to route out to the internet.
- We need to create two route tables: public and private to associate and route out the subnets.
- After creating the route tables we need to associate them to different subnets.
- After the associating them with subnets, we need to route out the public route table to the internet
- In the Edit routes section: 
  - 10.0.0.0/16 means: All of ip addresses can talk to each other and subnets can talk to subnets.
  - We add, 0.0.0.0 and add the internet gateway for this.

### What is NACL (Network access control list) ?
  
- NACL: Network access control list, is like a virtual firewall which protects subnet and it's stateless.
- Stateless means: If it allows something any request into the subnet -> It does not remember the sate so it does not allow it back out.
- So If there is an inbound rule, there should also be outbound rule.
- Most people leaves it default. And in default it open for all inbound and outbound services.
- Once use case can be, people use it to block an ip address at subnet level.

#### What is security Group?

- Security Group is a virtual firewall which protects our EC2 instance.
- Unlike NACL, Security Group is statefull. If there is some inbound rule and data comes in -> It remembers the state -> Allows the same rule out.