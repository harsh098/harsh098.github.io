---
title: "Projects"
date: 2022-06-13T20:55:37+01:00
draft: false
showDate : false
showDateOnlyInArticle : false
showDateUpdated : false
showHeadingAnchors : false
showPagination : false
showReadingTime : false
showTableOfContents : true
showTaxonomies : false 
showWordCount : false
showSummary : false
sharingLinks : false
showEdit: false
showViews: false
showLikes: false
layoutBackgroundHeaderSpace: false
---

I always try to find time to work and learn something new. Usually, most of these _pet-projects_ don't see the light of day. They are, however, great opportunities to try something in the real world and learn from it.
This is not an exhaustive list. There's a graveyard you can always visit on my [Github](https://github.com/harsh098).

## Pychanio

{{<badge>}}
Active
{{</badge>}}  
<br/>
{{<button href="https://harsh-mishra.gitbook.io/pychanio">}}
Go to Docs
{{</button>}}

{{<button href="https://pypi.org/project/pychanio/">}}
Get It On PyPI
{{</button>}}

An Opinionated Python Concurrency Library, that brings Golang like select-case DSL and channels to Python's asyncio. It offers a full interop with Python asyncio.

1. Supports both unidirectional and full-duplex channels.
2. Supports blocking and non blocking sends
3. Supports timeouts in select case blocks.
4. Support for nil channels
5. Support for sentinels to send signals such as HEARTBEAT, DONE, CANCEL etc.

### Source
{{< github repo="harsh098/pychanio" showThumbnail=true >}}  


## IaC and GitOps Pipeline for Kubeadm Cluster over Amazon EC2

{{<badge>}} Boilerplate {{</badge>}} {{<badge>}} Quickstart {{</badge>}}

This project allows users to create a GitOps Pipeline using Terraform for Infra Provisioning and Ansible for Configuration Management to create clusters using Kubeadm over Amazon EC2.


- Used Terraform for infrastructure provision and Ansible for configuration management.
- The Automation is handled via a Continuous Deployment (CD) Pipeline using GitHub Actions.
- The CD Pipeline uses Keyless Authentication to AWS via Github OIDC Provider for enhanced security
- Implemented the Least Privilege Principle Model to secure the Pipeline
- The Action publishes the Kubeconfig for connecting to the Kubernetes cluster to AWS Secrets Manager.
- Implemented Remote Terraform State Management using AWS S3 as the backend
- Modularized Terraform configuration into modules, which allows users to customize the deployment easily by editing the .tfvars files.
- The project uses Ansible Providers for Terraform to generate inventory files dynamically.

### Source
{{< github repo="harsh098/kubeadm-cluster-with-terraform-ansible-ec2" >}}


## Keyless AWS EKS Authentication Plugin for GitHub Actions

{{<badge>}}
Stable
{{</badge>}}

{{<badge>}}
Receives Occasional Updates
{{</badge>}}



A secure keyless sign-on solution for AWS EKS clusters within Github Actions CI/CD pipelines.
Addressed the lack of support for AWS CLI v2 in existing actions, enhancing security and future-proofing deployments.

{{<button href="https://github.com/marketplace/actions/helm-on-eks-with-aws-cli-v2-with-aws-iam-authenticator">}}
Get It On Github Marketplace
{{</button>}}


