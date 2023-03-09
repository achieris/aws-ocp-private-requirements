aws-ocp-requirements
=========

Ansible roles to create the required objects to install a private OCP cluster on AWS. 
The role create_aws_objects creates:
 - AWS VPC
 - 1 private subnet in each az of the selected region
 - 1 IGW
 - 1 NAT GATEWAY
 - 1 public subnet 
 - 2 route tables
 - 1 route53 private zone
 - 3 VPC endpoints
 - 1 security group (Open ports are 80, 443, 22 for world and all traffic for VPC cidr).

The role destroy_aws_objects all the previously listed resources.


Requirements
------------
Tested with Ansible 2.13.3

Required python modules: 

```sh
boto
boto3
botocore
```


create_aws_objects role variables
--------------
In create_aws_objects/vars/main.yml

```sh
ssh_key: 'ssh-key'

vpc:
  aws_vpc_name: "example-vpc-1"
  aws_vpc_region: "eu-central-1"
  aws_vpc_az_a: "eu-central-1a"
  aws_vpc_az_b: "eu-central-1b"
  aws_vpc_az_c: "eu-central-1c"
  aws_vpc_cidrblock: "10.26.0.0/24"

  private_subnets:
  - az: "eu-central-1a"
    name: private-subnet-a
    cidr: 10.26.0.0/26
  - az: "eu-central-1b"
    name: private-subnet-b
    cidr: 10.26.0.64/26
  - az: "eu-central-1c"
    name: private-subnet-c
    cidr: 10.26.0.128/26

  public_subnets:
  - az: "eu-central-1a"
    name: public-subnet-a
    cidr: 10.26.0.192/26

```

use create_aws_objects role
----------------

Export enviroment variables with AWS credentials:
```sh
export AWS_ACCESS_KEY_ID=YOUR_AWS_ACCESS_KEY_ID
export AWS_SECRET_ACCESS_KEY=YOUR_AWS_SECRET_ACCESS_KEY
```


Run ansible:
```sh
ansible-playbook create_objects.yaml
```

destroy_aws_objects role variables
--------------
In destroy_aws_objects/vars/main.yml

```sh
vpc:
  aws_vpc_name: "example-vpc-1"
  aws_vpc_region: "eu-central-1"

```

use destroy_aws_objects role
----------------

Export enviroment variables with AWS credentials:
```sh
export AWS_ACCESS_KEY_ID=YOUR_AWS_ACCESS_KEY_ID
export AWS_SECRET_ACCESS_KEY=YOUR_AWS_SECRET_ACCESS_KEY
```


Run ansible:
```sh
ansible-playbook destroy_objects.yaml
```

