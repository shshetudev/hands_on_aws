
## Flow at a glance

1. Create VPC
2. Define CIDR Block for VPC
3. Create Security Groups: Public, Private
4. Create Internet Gateway for the VPC -> Attach the Gateway to the VPC
5. Create Route table for each of the subnets: Public, Private -> Associate the route tables to different subnets -> Route out the public route table to the internet gateway

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


### What is Route table?

- We need to allow our route table on our public subnet, to route out to the internet
- We need to create two route tables: public and private to associate and route out the subnets
- After creating the route tables we need to associate them to different subnets
- After the associating them with subnets, we need to route out the public route table to the internet
- In the Edit routes section: 
  - 10.0.0.0/16 means: All of ip addresses can talk to each other and subnets can talk to subnets.
  - We add, 0.0.0.0 and add the internet gateway for this.