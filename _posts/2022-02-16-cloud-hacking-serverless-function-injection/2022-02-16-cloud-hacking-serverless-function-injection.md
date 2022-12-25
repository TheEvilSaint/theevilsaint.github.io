---
layout: post
title: 'Cloud Hacking :- Serverless Function Injection'
date: '2022-02-16'
description: 'In this example, we will demonstrate how to exfiltrate secrets through command injection against a serverless function. Serverless functions listen for events or triggers to be run. It is possible to inject data to these events, leading to injection vulnerabilities in serverless functions.'
coverimage: Cloud_Hacking_Serverless_Function_Injection.jpg
tags: cloud how-to serverless-functions
published: true
posttype: article
categories: article
---
In this example, we will demonstrate how to exfiltrate secrets through command injection against a serverless function. Serverless functions listen for events or triggers to be run. It is possible to inject data to these events, leading to injection vulnerabilities in serverless functions.

##What is a function?

It is a piece of code that you can use over and over again to perform a task.

##What is serverless?

In serverless architecture, you are building and running code on someone else's computer. In cloud context, the computer belongs to the cloud provider and resides on their premises. Because the computer serves your code, it is referred to as a server. The cloud provider is responsible for maintaining the server and, thus, you are left with a "serverless" setting.

##Piecing it together - A serverless function

A serverless function is a piece of code that runs on the cloud provider's computer and it can perform a task over and over again.

##What are some examples of serverless functions?

* AWS Lambda
* Microsoft Azure Functions
* Google Cloud Functions

To perform a task, the function requires an event or a trigger. These events or triggers can originate from different sources. Examples include:

* HTTP APIs
* Changes in systems like databases
* Other alerting systems

##Where does the injection come in?

In context of serverless functions, injection vulnerabilities occur when unexpected input is sent to the function. The process of sending unexpected input can be referred to as an injection attack. 

##Why does it work?

Two aspects come into play:

1. Your ability to control variables passed to the function;
3. Whether the server trusts your input and executes it.

In case of command injection vulnerabilities, the function should run shell or operating system commands in the background.

Now that we have our foundations set up, let us demonstrate this with an easy-to-follow example.

#Serverless event-data injection

This example demonstrates how to execute unwanted code via a serverless function. To do this example, you will only require a browser. We have done this example using the OWASP ServerlessGoat web application, which you can find in the below URL.

* https://www.serverless-hack.me/

The application converts Doc files to text from a URL. The output is then displayed on the screen.

<img src="/static/b576cf8f-2a9d-43b3-a1d1-3ed12f9b0f12.png">

In case of OWASP ServerlessGoat, the injection vulnerability occurs when the Doc filename is appended with code that gets executed. 

##Instructions

<h5 class="step">Step 1 - Navigate to https://www.serverless-hack.me/</h5>

The vulnerable app resides here. You can alternatively create your own serverless functions in the cloud!

<h5 class="step">Step 2 - Append the filename with the command you want to execute.</h5>

For example, using a semi-comma to separate the filename from the command you want to execute will print out the environment variables:
```
https://www.puresec.io/hubfs/document.doc;env
```

The output will display a secret (the AWS secret access key)!

<img src="/static/96002e52-8f1b-496a-ba9c-0c4631826a30.png">

Or to output text of your choice, you can use the "echo" command:
```
https://www.puresec.io/hubfs/document.doc;echo "hello"
```

Displayed below is "hello" echoed back to us.

<img src="/static/6359b91a-fa74-45f0-b9c2-52f5b3a49fae.png">


To understand why this works, you may wish to review the code used for the function:
```
const child_process = require('child_process');
const AWS = require('aws-sdk');
const uuid = require('node-uuid');

async function log(event) {
  const docClient = new AWS.DynamoDB.DocumentClient();
  let requestid = event.requestContext.requestId;
  let ip = event.requestContext.identity.sourceIp;
  let documentUrl = event.queryStringParameters.document_url;

  await docClient.put({
      TableName: process.env.TABLE_NAME,
      Item: {
        'id': requestid,
        'ip': ip,
        'document_url': documentUrl
      }
    }
  ).promise();

}

exports.handler = async (event) => {
  try {
    await log(event);

    let documentUrl = event.queryStringParameters.document_url;

    let txt = child_process.execSync(`curl --silent -L ${documentUrl} | /lib64/ld-linux-x86-64.so.2 ./bin/catdoc -`).toString();

    // Lambda response max size is 6MB. The workaround is to upload result to S3 and redirect user to the file.
    let key = uuid.v4();
    let s3 = new AWS.S3();
    await s3.putObject({
      Bucket: process.env.BUCKET_NAME,
      Key: key,
      Body: txt,
      ContentType: 'text/html',
      ACL: 'public-read'
    }).promise();

    return {
      statusCode: 302,
      headers: {
        "Location": `${process.env.BUCKET_URL}/${key}`
      }
    };
  }
  catch (err) {
    return {
      statusCode: 500,
      body: err.stack
    };
  }
};
```

##Well that was easy... What next?

Why not look into executing other commands or practice with creating your own serverless functions next?