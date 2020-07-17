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
1. When new code is committed, GitHub Actions can initiate code scanning to quickly and automatically find vulnerabilities and coding errors
1. Pull Requests trigger CI builds and automated testing using GitHub Actions. Secrets and credentials are encrypted at rest and obfuscated in log entries
1. The CI build in GitHub Actions builds the project and deploys it to App Service, while making changes to other cloud resources like service endpoints  
1. Azure Policy evaluates the Azure resources in the deployment and potentially denies the release, modifies the cloud resources, or creates warning events in the activity log
1. Azure Security Center identifies attacks targeting applications running in this project's deployments 
1. Continuous monitoring with Azure Monitor can revert (rollback) a commit based on monitoring data

## Components

* [Azure Active Directory](/azure/active-directory/fundamentals/active-directory-whatis) is a multi-tenant, cloud-based identity and access management service, controlling access to Azure and other cloud apps like M365 and GitHub
* [GitHub](https://docs.github.com/en/github) is where developers can collaborate with open source communities and within your organization (innersource)
* [Codespaces](https://docs.github.com/en/github/developing-online-with-codespaces/about-codespaces) is an online development environment, hosted by GitHub and powered by Visual Studio Code, that allows you to develop entirely in the cloud
* GitHub Security
  * [Vulnerability management](https://docs.github.com/en/github/managing-security-vulnerabilities) - when known vulnerabilities occur in your code (or in software packages your code uses), GitHub can raise an alert to the project, fork a new branch with updated code, and trigger a pull request to fix the vulnerability
  * [GitHub Dependabot](https://docs.github.com/en/github/administering-a-repository/about-github-dependabot) is an automated agent that checks for outdated packages and applications. Dependabot can update software dependencies or vulnerabilities to newer versions
  * Code scanning - will inspect code for known vulnerabilities and coding errors at scheduled times or after certain events occur (like a commit or a push)
* [GitHub Actions](https://docs.github.com/en/actions/getting-started-with-github-actions/about-github-actions) are custom workflows that provide continuous integration (CI) and continuous deployment (CD) capabilities directly in your code repository  
* [Azure Policy](/azure/governance/policy/overview) helps you manage and prevent IT issues with policy definitions that can enforce rules for your cloud resources
* [Azure Security Center](/azure/security-center/security-center-intro) provides unified security management and advanced threat protection across hybrid cloud workloads
* [Azure Monitor](/azure/azure-monitor/overview) collects and analyzes app telemetry (performance metrics, activity logs, etc.), and can identify conditions that require an alert to be sent to a human or another app

## Vulnerability Management

## Code Rollback

