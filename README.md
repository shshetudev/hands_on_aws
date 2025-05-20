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