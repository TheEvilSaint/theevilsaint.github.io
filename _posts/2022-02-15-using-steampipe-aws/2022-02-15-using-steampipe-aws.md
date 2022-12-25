---
layout: post
title: "Using Steampipe on AWS"
date: "2022-02-15"
description: "Steampipe is a tool that lets us gather information from AWS (or other sources) and lets us interact with that data the same way we would a relational database via SQL style queries."
coverimage: Using_Steampipe_on_AWS.jpg
tags: steampipe aws
published: true
posttype: article
categories: tutorial
---
Installing steam pipe 
```
sudo /bin/sh -c "$(curl -fsSL https://raw.githubusercontent.com/turbot/steampipe/main/install.sh)"
```

Checking the steam pipe version
```
steampipe -v
```

Installing steampipe plugin
```
steampipe plugin install steampipe
```


Making queries with steampipe
```
steampipe query "select name from steampipe_registry_plugin;"
```

Installing the AWS plugin
```
steampipe plugin install aws
```

Setting up AWS access keys and secrets for Single Sign On environments
```
aws configure sso
```


Configuring AWS access keys and secrets for standard environments
```
aws configure
```

Configuring the AWS configuration file for steampipe. 
```
nano ~/.steampipe/config/aws.spc
```

Dropping down into an interactive query session. 
```
steampipe query
```

Listing tables that are available
```
.tables
```

Inspecting a table to see what columns it has
```
.inspect aws_iam_role
```


Example query 
```
select 
  group_name,
  group_id
from
  aws_vpc_security_group_rule
where 
  type = 'ingress'
  and cidr_ip = '0.0.0.0/0';
.quit
```

Running compliance checks involves an additional module
```
git clone https://github.com/turbot/steampipe-mod-aws-compliance.git
```

To run the following commands we need to enter the newly created directory
```
cd steampipe-mod-aws-compliance
```

We can either run all of the checks
```
steampipe check all
```

Or specfic versions. 
```
steampipe check benchmark.cis_v140
```

If needed we can even export a report in html and csv format
```
steampipe check benchmark.cis_v140 --export=report.html --export=report.csv
```