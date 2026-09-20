# Secure-AI-solutions-in-the-Cloud-Lab
Hands-on lab securing an AI solution in Azure: RBAC access control and a Microsoft Foundry content filter with prompt shields and a blocklist.

# Secure AI Solutions in the Cloud Lab (Azure RBAC, Microsoft Foundry Content Filters and Prompt Shields)

`Microsoft Azure` · `Microsoft Foundry` · `Azure AI Content Safety` · `RBAC` · `Content Filtering` · `Prompt Shields` · `Microsoft Applied Skills`

## Overview
This lab was hands-on practice with **securing an AI solution in Azure**, completed as part of the Microsoft Applied Skills credential *Secure AI solutions in the cloud*. It covered two layers of protection: controlling **who can access** a Microsoft Foundry resource using Azure role-based access control (RBAC), and controlling **what content a model can accept and return** by building a content filter with harm categories, prompt shields and a custom blocklist.

Rather than just clicking through the steps, I documented each setting as I went and captured the review page before creating the filter, so I could check the final configuration against what I intended instead of trusting memory.

## Objective
Get comfortable assigning access to an AI resource in the Azure portal, and configuring a content filter in Microsoft Foundry that blocks harmful input, defends against jailbreak attempts, and stops a specific term from being sent to the model.

## Environment
- **Lab platform:** Microsoft Learn Applied Skills assessment, using a temporary lab environment provisioned for the session
- **Portal / tools:** Azure portal and Microsoft Foundry (Guardrails + Controls)
- **Foundry resource:** `user1-65287604-resource`
- **Foundry project:** `project-65287604`
- **Lab user:** `User1-65287604`
- **Date completed:** 20 September 2026

Lab account and resource IDs are auto-generated for each lab launch, so they only exist for the duration of the session.

## Tools I Used

| Tool | What It Does | Why I Used It |
|------|--------------|----------------|
| **Azure portal** | Azure management interface | Used Access control (IAM) to manage who can access the Foundry resource |
| **Azure RBAC** | Role-based access control | Assigned a role to the lab user so access was granted through a role assignment |
| **Microsoft Foundry** | AI app and model platform | Hosted the project and the Guardrails + Controls area where content filters are built |
| **Content filters** | Harm-category filtering (powered by Azure AI Content Safety) | Blocked unwanted content at both the input and output stages |
| **Prompt Shields** | Detection of prompt attacks | Defended against jailbreak attempts and flagged indirect attacks |
| **Input blocklist** | Custom term blocking | Stopped a specific term (`ProjectX`) from being passed to the model |

## What I Did

### Granting Access with Azure RBAC
1. Opened the Foundry resource in the Azure portal and went to **Access control (IAM)**.
2. Clicked **Add**, then **Add role assignment**.
3. In the member picker, searched for `user1-65287604`. The results included the user account as well as entries for the Foundry resource and its project, so I had to pick the correct entry.
4. Selected the `User1-65287604` user account, confirmed it appeared under **Selected members**, and clicked **Select**.
5. Role assigned: **[ADD THE ROLE NAME FROM YOUR LAB INSTRUCTIONS]**

### Building the Content Filter
1. In Microsoft Foundry, opened **Guardrails + Controls** and started **Create content filter**. The wizard has five steps: Basic information, Input filter, Output filter, Connection and Review.
2. Named the filter `Filter1`.
3. Configured the **input filter** with these settings:

| Category | Media type | Action | Blocking threshold |
|----------|-----------|--------|--------------------|
| Violence | Text, Image | Annotate and block | Lowest blocking |
| Hate | Text, Image | Annotate and block | Lowest blocking |
| Sexual | Text, Image | Annotate and block | Medium blocking and Lowest blocking |
| Self-harm | Text, Image | Annotate and block | Lowest blocking |
| Prompt shields for jailbreak attacks | Text | Annotate and block | Jailbreak attacks will be blocked |
| Prompt shields for indirect attacks | Text | Annotate only | Content will be annotated but not blocked |

4. Added `ProjectX` to the **input blocklist** and left **spotlighting** set to Disabled.
5. Completed the **output filter** step: **[ADD OUTPUT FILTER SETTINGS IF YOU HAVE A SCREENSHOT, OR DELETE THIS LINE]**
6. On the **Review** page, checked the full configuration (filter name, input filter table, blocklist) before clicking **Create filter**.

A note on the thresholds: the slider labels explain what each level does. Lowest blocking blocks only the most severe unwanted content, while Medium blocking blocks both moderate and highly severe content.

### Verifying the Result
1. Back on the content filters list, `Filter1` appeared with a modified timestamp of **Sep 20, 2026, 9:09 AM**, confirming it was created.
2. The **Applied deployment** column for `Filter1` was empty, meaning the filter existed but had not been attached to a model deployment.
3. The Microsoft Learn results page then showed the credential as earned, with a **Pass** and an overall result of **67%** (67% needed to pass).

## What's in This Repo

```
secure-ai-cloud-lab/
├── README.md                                   # This file
└── screenshots/
    ├── 01-iam-add-role-assignment-member.png
    ├── 02-input-filter-violence-hate-sexual.png
    ├── 03-input-filter-self-harm-prompt-shields.png
    ├── 04-input-blocklist-and-output-filter.png
    ├── 05-review-basic-information.png
    ├── 06-review-input-filter-table.png
    ├── 07-filter1-in-content-filters-list.png
    └── 08-applied-skills-credential-earned.png
```

## Skills I Picked Up
- **Assigning access through RBAC,** using Access control (IAM) and the member picker to grant a specific user access to an AI resource, and paying attention to which entry I was selecting when the search returned users and resources together.
- **Configuring layered content filtering,** setting per-category actions and thresholds for violence, hate, sexual content and self-harm, and understanding what each blocking level actually blocks.
- **Using Prompt Shields,** blocking jailbreak attacks while choosing annotate-only for indirect attacks, and understanding that "annotate" records the detection without stopping the request.
- **Blocking specific terms with a blocklist,** which handles cases the built-in harm categories don't, like keeping an internal project name out of prompts.
- **Reviewing before committing,** using the wizard's Review step to check every setting against my intent before creating the filter.
- **Separating "created" from "applied,"** noticing that a filter in the list isn't protecting anything until it's attached to a deployment.

## How This Applies in the Real World
Securing an AI solution isn't one control, it's several layers working together. RBAC decides who can reach the resource at all, and content filters decide what the model will accept and produce once someone does. Both are standard parts of deploying AI in a business setting, where the concerns are unauthorized access, harmful output, prompt injection and accidental leakage of sensitive information.

The blocklist is a good small example of the leakage concern: a term like an internal project codename is not "harmful" in any content-safety category, so it needs its own explicit rule. Likewise, the prompt shield settings reflect the real-world risk that users, or documents the model reads, will try to steer the model away from its instructions.

## Where I'm Coming From
I'm making the jump into cybersecurity from a background in **healthcare**. It's a different field on paper, but a lot of the muscle memory carries over: following procedures carefully, protecting sensitive information, staying calm and methodical when something isn't working the way it's supposed to. I'm currently studying for **CompTIA Security+** and building labs like this one to get real hands-on reps in, since that's what I'm missing on paper right now compared to my experience.

I'll be upfront, I got a lab question wrong on my first attempt here, and that's included above rather than cleaned up, because I'd rather show the real process than a version that pretends everything landed correctly the first time.

## What I Want to Learn Next
- Attaching a content filter to a model deployment and testing it in the playground to see blocked and annotated responses
- Comparing stricter and more permissive thresholds and how they change model behavior
- Working with more RBAC roles and understanding least privilege for AI resources
- Adding monitoring and logging so blocked content and prompt attacks can be reviewed

## Limitations & What I'd Do Differently in Production
- **The filter was not applied to a deployment.** The list showed no applied deployment for `Filter1`, so it wasn't actively filtering any model traffic. In production the filter would be attached and then tested.
- **Most categories used the lowest blocking threshold,** which only blocks the most severe content. A production deployment would likely use stricter thresholds, chosen based on the audience and use case.
- **Indirect prompt attacks were annotate-only.** That records the detection but doesn't stop it. Production systems handling external documents would usually block them.
- **No testing was performed.** I configured the filter but didn't send test prompts to confirm it behaved as expected.
- **The lab environment is temporary.** Accounts, IDs and resources were provisioned for the session and don't reflect a real tenant's permissions structure.

## References
- [Microsoft Applied Skills: Secure AI solutions in the cloud](https://learn.microsoft.com/en-gb/credentials/applied-skills/secure-ai-solutions-in-the-cloud/)
- [What is Azure role-based access control (Azure RBAC)?](https://learn.microsoft.com/azure/role-based-access-control/overview)
- [What is Azure AI Content Safety?](https://learn.microsoft.com/azure/ai-services/content-safety/overview)
