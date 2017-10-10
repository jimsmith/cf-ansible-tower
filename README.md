# Installation Guide: Ansible Tower Install (3.1.5)

Author: Ritesh Patel | Last Updated: Oct 09, 2017

## Overview

This guide supports the access, management, and deployment of the Ansible Tower from CloudFormation templates in Amazon Web Services

## Getting Started

The following sections provide guidance for using the template to configure and launch resources for Ansible Tower.

### Accessing the Templates

Main template file is called "ansible-tower.template". In the repository there is a bash
script to execute and tear down the stack. (if you wish to do it through AWS CLI, now of course you must install / configure CLI, duh!)

### Template File Structure

The master template repository is represented by the following tree Structure
```
├──
├── README.md
├── create-ansible-tower-stack.sh
└── ansible-tower.template

```

#### Known Dependencies

This template does not create VPC, Subnets or RDS Subnet Groups. Template user must create these resources beforehand and replace VPC, Subnet and RDS Subnet group references with the actual values from the environment. Additionally, keypair must be modified to reflect the correct keypair from user's environment.

#### Parameters and Conditions

The templates use parameters to allow for dynamic provisioning of the related resources, allowing custom network, computer, and storage controls to be applied at runtime. The following table lists the known parameters and relevant conditions utilized by the resources during provisioning.

__Parameters__

| Parameter Identifier | Default Value |
|----------------------|---------------|
| pApplicationIdentifier | ansibletower |
| pApplicationName | Ansible Tower |
| pEC2AvailabilityZoneTarget | us-east-1a (replace to your region) |
| pEC2InstanceType | m4.large |
| pEC2Keypair | keypair.pem |
| pServerAMI | ami-c998b6b2 |
| pAnsibleSubnet | subnet-5c3f702b (must be replaced) |
| pAnsibleVPC | vpc-84b702fd (must be replaced) |
| pEnvironment | development |
| pDatabaseAvailabilityZone | |
| pDatabaseInstanceClass | db.m4.large |
| pDatabaseAdmin | ansibleAppDBAdmin |
| pDatabasePassword | (check template) |
| pDatabaseName | ansibletowerdb |
| pDatabasePort | 5432 |
| pDatabaseSubnetGroupName | db-subnet-group-name (must be replaced) |
| pRabbitMQPort | 5672 |
| pRabbitMQVHost | tower |
| pRabbitMQUserName | tower |
| pRabbitMQPassword | password |
| pRabbitMQCookie | cookiemonster |
| pAnsibleTowerPassword | (check template) |
| pRoleName | prd-ansible-tower-role |
| pTagProductOwner | Arpit Shah |
| pCloudWatchActionsPolicy | | |  |
| pTagTechnicalContact | Ritesh Patel |
| pUniqueTimestampHash | 123456789 |
| pCloudWatchEventsPolicy | default |
| pCloudWatchInlinePolicyName | |

__Conditions__

| Condition Identifier | Parameters Referenced | Logical Operator Purpose |
|----------------------|----------------------|--------------------------|
| cLaunchAsDedicatedInstance | pVPCTenancy | Evaluates if EC2 instances should utilize dedicated hosts. <br><br>_Logical operation:_ If the tenancy parameter matches the string 'dedicated', return true |

### Resources Created by this Template

The following table lists the known resource types and identifiers created during runtime provisioning.

| Resource Name | Type |
|---------------|------|
| rAnsibleEIP |  Elastic IP Address for Ansible Tower |
| rIAMRole | IAM role with cloudwatch policies |
| rIAMInstanceProfile | Instance profile to attach with Ansible Tower instance |
| rELBSecurityGroup | Load balancer security group |
| rAnsibleSecurityGroup | Ansible Tower security group |
| rAnsibleTower | Ansible Tower instance |
| rELBAnsibleTower | Ansible Tower load balancer |
| rDatabaseSecurityGroup | Database security group |
| rRDSAnsibleTower | RDS database instance for Ansible Tower |

### Wait conditions

This template creates one wait condition to track the completion of rAnsibleTower instance. Wait condition has a 20 minutes of timeout. In the event of a successful completion the wait condition receives a signal and marks the resource creation as complete else the entire stack is rolled back after 20 minutes.

### Required Ports and Protocols

The application requires the following ports and protocols for successful installation and configuration steps. All of the ports and protocols identified are currently provisioned and managed through the resources created by this template. Please see (this list)[#resources-created-by-this-template] for more information.

| Port | Protocol | Purpose | Notes |
|------|----------|---------|-------|
| 80 | TCP | Port for the Web UI |

### Template modifications

To use this template, you must replace following parameters with the actual values from your environment.

| Parameters to be replaced |
|-----------|
| pAnsibleVPC |
| pAnsibleSubnet |
| pDatabaseSubnetGroupName |
| pServerAMI |
| pKeypair |

### Known Issues and Bugs
Gimme a holler if you find one :D :D :D
