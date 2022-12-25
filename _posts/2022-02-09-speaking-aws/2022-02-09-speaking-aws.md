---
layout: post
title: 'Speaking AWS'
date: '2022-02-09'
description: 'aws, cloud, s3-buckets, scoutsuite'
coverimage: Speaking_AWS.jpg
tags: aws cloud s3-buckets scoutsuite
published: true
posttype: article
categories: article
---
# Introduction To Terminology


| AWS Term                             | Pentester Analogy                                                                           |
|--------------------------------------|---------------------------------------------------------------------------------------------|
| Region                               | Geographical location containing availability zones                                         |
| AZ (Availability Zone)               | Location mapped to a physical data centre in a region                                       |
| VPC (Virtual Private Cloud)          | The network infrastructure (can include public and private subnets)                         |
| EC2 (Elastic Compute Cloud) instance | Virtual Machine                                                                             |
| Security Groups                      | Firewall rules applied to the single instance                                               |
| Network ACL                          | Firewall rules applied to a subnet                                                          |
| ELB (Elastic Load Balancer)          | Network load balancer to optimise traffic across instances.                                 |
| IAM (Identity & Access Management)   | A service to manage users, groups, roles and security policies                              |
| S3 (Simple Storage Service Bucket)   | A container for any type of data                                                            |
| CloudTrail & CloudWatch              | Logging, Monitoring and Auditing of Events.                                                 |
| RDS (Relational Database Service)    | A service allowing the creation of relational databases (MS SQL, Oracle, Aurora/MySQL, etc) |
| KMS (Key Management Service)         | KeyChain, Password Vault, etc                                                               |

https://cloudonaut.io/aws-security-primer/


# Orientation 

Configure the CLI for first usage
```
aws configure --profile <profile name>
```

> It should be noted that the credentials will be stored in your home directory: `~/.aws/credentials`

List AWS regions & VPC(s) available
```
aws ec2 describe-regions
aws ec2 describe-vpcs
```

Get AWS console alias
```
aws iam list-account-aliases
```

Get username associated to an AWS API KEY
```
aws iam get-user
```

If you have no access to IAM then the following command will work.
```
aws sts get-caller-identity 
```

## Getting Your Whereabouts


Find Your ID
```
aws sts get-caller-identity
```

Log in to the console
```
https://Your_Account_ID.signin.aws.amazon.com/console/
```

## Setting Up Temporary Credentials

To use the credentials returned by the assume-role 

In Linux set some bash variables with export
```
$ export AWS_ACCESS_KEY_ID=XXXX
$ export AWS_SECRET_ACCESS_KEY=XXXX
$ export AWS_SESSION_TOKEN=XXXX
```

In Windows do the equivalent with SET
```
C:\> SET AWS_ACCESS_KEY_ID=XXXX
C:\> SET AWS_SECRET_ACCESS_KEY=XXXX
C:\> SET AWS_SESSION_TOKEN=XXXX
```

> Note that this will set the credentials for your default AWS account in your machine. This is similar to running `aws configure` without specifying a profile name. 

Security Groups vs Network ACLs

| Security Group                                                                                                                                                | Network ACL                                                                                                                                            |
|---------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------|
| First layer of defence                                                                                                                                        | Second Layer of defence                                                                                                                                |
| Operates at instance level                                                                                                                                    | Operates at subnet level                                                                                                                               |
| Supports allow rules only                                                                                                                                     | Supports allow and deny rules                                                                                                                          |
| "stateful": Return traffic is automatically allowed regardless of any rules                                                                                   | "stateless": Return traffic must be explicitly allowed by the rules                                                                                     |
| All rules are evaluated before deciding if traffic is allowed                                                                                                 | Rules are evaluated in order when deciding to allow traffic                                                                                            |
| Applies to an instance only if someone specifies the security group when launching the instance, or associates the security group with the instance later on. | Automatically applies to all instances in the subnets it's associated with (therefore, you don't have to rely on users to specify the security group). |

## Installing AWS CLI




## Enumerating With AWS CLI



### S3 Buckets


```
aws sts assume-role --role-arn arn:aws:iam::093313834310:role/pentesterRole --role-session-name pentesterRole --profile assessment
```


Listing Buckets
```
aws s3 ls 
aws s3api list-buckets 
```


Copy File From Bucket To Current Directory
```
aws s3 cp s3://this-will-be-the-bucket-name/the-name-of-the-file.txt . 
```



## Networking Configuration Review 

### Using Cloud Mapper

<h5 class="step">Edit the config.json file to enter the AWS ID</h5>

We can edit the config file via the `cloudmapper.py` python script. 
```
python cloudmapper.py configure add-account --config-file <config.json> --name <arbitrary name> --id <AWS Account ID>
```

Or we can just directly edit the config.json file using your prefered editor. Which of course is `vi` ;-)
```
vi config.json
```

<h5 class="step">Prepare and start the Web Server</h5>


First we prepare
```
python cloudmapper.py prepare --config <config.json> --account <arbitrary_name>
```

Then we start
```
python cloudmapper.py webserver
```

## ScoutSuite


Installing ScoutSuite
```
$ git clone https://github.com/nccgroup/ScoutSuite
$ cd ScoutSuite
$ virtualenv -p python3 venv
$ source venv/bin/activate
$ pip install -r requirements.txt
$ python scout.py --help
```

We can run ScoutSuite in the following way. We use the `--profile` to specify API credentials to use. 
```
scout.py --provider aws --profile <profile_name>
```

> Specifying the provider type (aws) and credentials is mandatory. 

We can specify a report directory to store results with the `--report-dir` flag. 
```
scout.py --provider aws --profile <profile_name>  --report-dir <folder>
```

We can specify which regions we want ScoutSuite to look at with the `--regions` flag. 
```
scout.py --provider aws --profile <profile_name> --region us-east-1,eu-west-1
```

We can limit the services we want to check for
```
scout.py --provider aws --profile <profile_name> --services iam,s3
```

There are other flags that may be of interest which can be found by looking at the help `--help`
```
scout.py --help
```

## Priv Escalation


Pmapper tool by NCC 


### Pacu


<a href="https://github.com/RhinoSecurityLabs/pacu" target="_blank" class="external" title="Pacu Tool By Rhino Security Labs" rel=”nofollow”>Pacu Tool By Rhino Security Labs</a>


Installing Pacu
```
pip3 install pip
pip3 install -U pacu
pacu
```

Enumeration of the root user. Can be done manually but after a few attempts, it requires a captcha. 


Enumeration of Account ID via 
```
IAM -> Roles > Elevate-S3 > Edit trust policy
```

The team over at Rhino Security have a great write up on how to enumerate users via this process. <a href="https://rhinosecuritylabs.com/aws/aws-iam-user-enumeration/" target="_blank" class="external" title="AWS IAM User Enumeration" rel=”nofollow”>Rhino Security Labs - AWS IAM User Enumeration</a>


## Steam Pipe


Running SteamPipe
```
steampipe check benchmark.cis_v140 --export=report.html --export=report.csv
```


## Prowler


Running Prowler. Use the `-p` flag to specify API profile to use
```
./prowler -p <profile_name>
```

Use `ansi2html` if you want an HTML report
```
pip install ansi2html
./prowler -p <profile_name> | ansi2html -la > report.html
```

## Assessing AWS

> The following is a very generic methodology you can use until you refine your own approach via experience. 

* Launch Scout Suite to collect configuration data and initial list of issues
* Launch prowler and collect list of issues
* Review Trusted Advisor (if available for the account)