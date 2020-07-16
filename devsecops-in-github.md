---
title: DevSecOps in GitHub
titleSuffix: Azure Solution Ideas
author: fmigacz
ms.date: 07/15/2020
description: Making security principles and practices an integral part of DevOps while maintaining improved efficiency and productivity.
ms.custom: acom-architecture
ms.service: architecture-center
ms.category:
  - devops
  - hybrid
ms.subservice: solution-idea
ms.author: fmigacz
social_image_url: /azure/architecture/solution-ideas/articles/media/devsecops-in-github.png
---

# DevSecOps in GitHub

DevSecOps involves utilizing security best practices from the beginning of development, shifting the focus on security away from auditing at the end and towards development in the beginning using a shift-left strategy. GitHub has tools that support every part of a DevSecOps workflow, and is a superb service for building cloud solutions.

![Architecture Diagram](../media/devsecops-in-github.png)
*Download an [SVG](../media/devsecops-in-github.svg) of this architecture.*

## Data flow

1. Developers accessing GitHub resources are redirected to Azure AD for authentication using the SAML protocol. The organization enforces Single Sign-On (SSO) using FIDO2 strong authentication with the Microsoft Authenticator app
1. Developers access a Codespace (a prebuilt development environment) to begin working on tasks. The Codespace is a container that contains their IDE, all necessary dependencies, and any required security scanning tools installed
1. Developers make changes to local git clones of projects, and commit their changes back to the GitHub code repository. Developer teams manage projects, tasks, and progress using Project Boards
1. When new code is committed, GitHub Actions can initiate code scanning to quickly and automatically find vulnerabilities and coding errors
1. Pull Requests trigger CI builds and automated testing using GitHub Actions. Secrets and credentials are encrypted at rest and obfuscated in log entries
1. The CI build in GitHub Actions builds the project and deploys it to App Service, while making changes to other cloud resources like service endpoints  
1. Azure Policy evaluates the Azure resources in the deployment and potentially denies the release, modifies the cloud resources, or creates warning events in the activity log
1. Azure Security Center identifies attacks targeting applications running in this project's deployments 
1. Continuous monitoring with Azure Monitor can revert (rollback) a commit based on monitoring data

## Components

* [Azure Active Directory](/azure/active-directory/fundamentals/active-directory-whatis) provides identity and access management services for your organization, allowing control over access to the resources inside Azure, GitHub Enterprise, and Azure DevOps.
* Codespaces
* Source code is hosted on [GitHub Enterprise](https://help.github.com/en/github), where developers can collaborate within your organization and the open source communities. GitHub Enterprise offers advanced security features to identify vulnerabilities in the code you write and in open source dependencies
* GitHub Security
* GitHub Actions
* [Azure Policy](/azure/governance/policy/overview) lets you create, assign, and manage policies. These policies enforce different rules and effects over your resources, so those resources stay compliant with your corporate standards and service level agreements. It integrates with Azure Kubernetes Service too.
* Azure Security Center
* Using [Azure Monitor](/azure/azure-monitor/overview) lets you get insights on the availability and performance of your application and infrastructure. It also gives you access to signals to monitor your solution's health and spot abnormal activity early.

## Vulnerability Management

## Code Rollback

