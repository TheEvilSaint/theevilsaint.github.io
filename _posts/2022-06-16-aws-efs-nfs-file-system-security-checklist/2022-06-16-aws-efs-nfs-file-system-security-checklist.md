---
layout: post
title: 'AWS EFS NFS file system Security Checklist'
date: '2022-06-16'
description: 'AWS EFS (NFS file system); Quick Security Hardening Guide - This acts as a Quick Checklist to configuring your AWS EFS securely. This is Amazon’s implementation of the network file system or NFS protocol for Amazon’s cloud customers.'
coverimage: 
tags: aws cloud
published: true
posttype: article
categories: article
---

# AWS EFS (NFS file system); Quick Security Hardening Guide

No pun intended!

This acts as a Quick Checklist for securing your AWS EFS Instances. EFS is Amazon’s implementation of the network file system or NFS protocol for Amazon’s cloud customers. In less than four steps!

The information provided is my own advise. I hope to provide you with a better understanding about AWS EFS and how you can use it securely. See the external references for supporting material.

Let's get configuring!

## Checklist 

	1.	Encryption : enabled by default, however consider checking it is configured securely. Refer to Amazon if in doubt.
	2.	Network access: make sure that default security groups have been removed; only security groups created by you can access the mounted file system(s). This limits the file system access to only relevant parties. 
	3.	Consider using unique SSH keys to access each Instance: an Amazon EFS instance. Compromised keys cannot be reused on multiple machines (i suspect you are using unique and strong SSH keys)
	4.	See other ideas in the external references.  

### References:

Edureka!, AWS EFS Tutorial: https://m.youtube.com/watch?v=yKhE2hzlbqA
Amazon, on AWS EFS security: https://docs.aws.amazon.com/efs/latest/ug/security-considerations.html