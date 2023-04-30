# Launch EC2 Instance in VPC

Here are the steps to launch an EC2 instance in a Virtual Private Cloud (VPC) on AWS:

1. Create a VPC
```
aws ec2 create-vpc --cidr-block 10.0.0.0/16
```

2. Create a subnet within the VPC
```
aws ec2 create-subnet --vpc-id <vpc-id> --cidr-block 10.0.0.0/24
```

3. Create an internet gateway
```
aws ec2 create-internet-gateway
```

4. Attach the internet gateway to the VPC
```
aws ec2 attach-internet-gateway --vpc-id <vpc-id> --internet-gateway-id <gateway-id>
```

5. Create a route table for the VPC
```
aws ec2 create-route-table --vpc-id <vpc-id>
```

6. Add a route to the route table to allow traffic to the internet gateway
```
aws ec2 create-route --route-table-id <route-table-id> --destination-cidr-block 0.0.0.0/0 --gateway-id <gateway-id>
```

7. Associate the route table with the subnet
```
aws ec2 associate-route-table --subnet-id <subnet-id> --route-table-id <route-table-id>
```

8. Create a security group for the instance
```
aws ec2 create-security-group --group-name my-security-group --description "My security group" --vpc-id <vpc-id>
```

9. Authorize inbound traffic to the security group
```
aws ec2 authorize-security-group-ingress --group-id <security-group-id> --protocol tcp --port 22 --cidr 0.0.0.0/0
```

10. Launch an EC2 instance in the subnet with the specified security group and key pair
```
aws ec2 run-instances --image-id <ami-id> --count 1 --instance-type t2.micro --key-name <key-pair-name> --security-group-ids <security-group-id> --subnet-id <subnet-id>
```

Make sure to replace `<vpc-id>`, `<subnet-id>`, `<gateway-id>`, `<route-table-id>`, `<security-group-id>`, `<ami-id>`, and `<key-pair-name>` with the actual values for your VPC, subnet, internet gateway, route table, security group, Amazon Machine Image (AMI), and key pair name.