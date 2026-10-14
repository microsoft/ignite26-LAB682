<!--
Lab: LAB682
Title: Govern AI agents across their lifecycle
Instruction set ID: TBD
Language: en-US
Source version: 0.1
Status: Combined Skillable draft; Sections 1 and 2 are sourced from the current learner instructions, and Section 3 is a placeholder pending authoring and tenant validation
Last updated: 2026-10-13
Sources: ALIGNMENT_PLAN.docx, lab-01-manage-agent-identities.md, lab-02-manage-agent-permissions.md, lab-03-automate-agent-sponsorship.md, and LAB-04-GSA-SOLUTION-BRIEF.md

AUTHORING STRUCTURE
- Upload this file to Skillable as one instruction set.
- Preserve the alignment plan's three-section structure:
  1. Manage agent identities at scale.
  2. Govern agent identities and lifecycle.
  3. Protect agent access to resources.
- Section 2 contains two explicitly labeled governance sessions because the permission and sponsorship journeys were validated separately.
- Sections 1 and 2 each end with a section or session checkpoint rather than a lab completion page.
- Only the final section should end with Complete the lab after Section 3 learner instructions are authored.
- Do not publish this combined draft to learners until the Section 3 placeholder is replaced and the complete file is validated in Skillable.

REQUIRED LAB VARIABLES
- estimatedDuration: Total learner-facing duration for the combined lab; target value is 60 minutes.
- LCW_AgentName: Unique display name of the learner's assigned Agent ID; shared by Section 1 and Session 2A.
- LCW_AgentID: Object ID of the learner's assigned Agent ID.
- LCW_agent365URL: Direct Microsoft Agent 365 URL for the learner's assigned agent.
- LCW_blueprintName: Agent identity blueprint associated with LCW_AgentName.
- LCW_AgentNamePermissionURL: Direct URL to the learner's agent permission view.
- LCW_accessPackageURL: Direct URL to the Weather API access-package request page.
- governedAgentName: Agent whose sponsorship is transferred in Session 2B.
- currentSponsorName: Initial sponsor of governedAgentName.
- newSponsorName: Manager who becomes the new sponsor.

ENVIRONMENT SETUP REQUIRED BEFORE RELEASE
1. Configure all required lab variables and verify that none render as blank.
2. Provide the licensing and least-privileged roles required for Microsoft Agent 365, Microsoft Entra Agent ID, Lifecycle Workflows, Outlook, My Access, and the future Global Secure Access activity.
3. Pre-provision at least one unique Agent ID per simultaneous learner plus replacement capacity.
4. Assign each learner as owner and sponsor of LCW_AgentName so Agent 365, the Weather API service, and My Access can discover it.
5. Associate LCW_AgentName with LCW_blueprintName and configure inherited delegated Weather.Read access for Weather API.
6. Leave LCW_AgentName without Weather.Read.All at the beginning of the lab.
7. Configure the **Weather API Access (App Permission)** package to grant Weather.Read.All through automatic approval.
8. Ensure the Weather API service supports Enable OBO, On-Behalf-Of, Autonomous, request access here, and deterministic post-grant token refresh.
9. Set currentSponsorName as the sponsor of governedAgentName and set newSponsorName as the manager of currentSponsorName.
10. Configure Offboard Agent Sponsors (Real-time) with Send email to manager about sponsorship changes and Transfer agent sponsorships to manager.
11. Provide the learner an active Lifecycle Workflows Administrator role and an Outlook inbox that can receive the workflow notification.
12. Isolate every learner's agents, permission request, workflow run, and notification.
13. Define reset procedures for permission state, access-package assignments, sponsorship, workflow history, and mailbox state.
14. Complete the Section 3 Global Secure Access environment, learner instructions, and validation before publication.
-->

# LAB682: Govern AI agents across their lifecycle

AI agents can spread across platforms, access organizational resources, and continue operating as people change roles or leave the organization. In this lab, you will discover and inspect agent identities, govern their permissions and lifecycle, and explore how organizations can protect agent access to resources.

**Estimated duration:** @lab.Variable(estimatedDuration)

## Learning objectives

After completing this lab, you will be able to:

- Discover agents across platforms by using Microsoft Agent 365.
- Inspect an Agent ID, its blueprint, and inherited delegated permissions.
- Compare delegated on-behalf-of access with autonomous application access.
- Request missing agent access through a governed access package.
- Use a lifecycle workflow to preserve accountable agent sponsorship.
- Explain how Global Secure Access can provide network access controls to an agent's web access.

## Prerequisites

To complete this lab, you should be familiar with:

- The Microsoft 365 and Microsoft Entra admin centers.
- Basic identity, permission, and access-governance concepts.
- Delegated and application authorization.
- The purpose of agent owners, sponsors, and identity lifecycle workflows.

> [!Note]
> The lab environment is preconfigured and shared with other attendees. Use only the identities, agents, applications, workflows, and resources specified in these instructions.

===

# Getting started
Login to the virtual machine with an Entra ID account. 

1. [] Select **Other user** in the bottom left corner and sign in with the following credentials.

Username: +++@lab.CloudCredential(Ignite2026-Lab682).EntraUsername+++

Password: +++@lab.CloudCredential(Ignite2026-Lab682).EntraPassword+++

2. [] If **Other user** does not appear, select the power button in the bottom right corner and select **Restart**.
3. [] Once the VM has restarted, you should see **Other user** available.

===

# Section 1: Manage agent identities at scale

**Section 1 of 3 | Estimated time: 20 minutes**

Organizations can accumulate AI agents across multiple platforms faster than administrators can inventory and govern them. In this section, you will assess the organization's agent estate, locate your assigned agent, inspect its identity and security information, and continue into Microsoft Entra to review its Agent ID, blueprint, and inherited permissions.

In this section, you will:

- Use Agent Overview and Agent Map to assess agent scale and distribution.
- Locate your assigned agent in the Agent Registry.
- Inspect the agent's identity, permissions, and security protections.
- Review the Agent ID and its blueprint in Microsoft Entra.

===

# Assess the agent estate

In this task, you will use Agent Overview to identify the scale and governance condition of the organization's agent estate.

1. [] In a new browser tab, open the [Microsoft 365 admin center](https://admin.cloud.microsoft), and then sign in with the below credentials.

Username: +++@lab.CloudCredential(Ignite2026-Lab682).EntraUsername+++

TAP: +++@lab.CloudCredential(Ignite2026-Lab682).TAP+++

2. [] In the left navigation menu, select **Agents**, and then select **Overview**.
4. [] If the **Overview stats at a glance** tour appears, close it.
5. [] Review the **Agent registry**, **Active users**, **Agent run-time**, **Connected platforms**, and **Tools** metrics.
6. [] Review **Top actions for you**, **Agent analytics**, and **Trending agents by active users**.
7. [] Confirm that the overview identifies agents that require administrative attention.

> [!Knowledge]
> Agent Overview brings inventory, activity, and governance insights together so administrators can prioritize investigation instead of reviewing agents one at a time.

===

# Visualize agent sprawl

In this task, you will use Agent Map to understand where the organization's agents originate and how administrators can move from the estate-wide view to individual agents.

1. [] In the left navigation menu, expand **Agents**, and then select **All agents**.
1. [] On the **All agents** page, select **Map**.
1. [] If the **Your agents, at a glance** tour appears, close it.
1. [] Review how Agent Map groups agents by creation platform.
1. [] Review the **Total agents**, **Agents at risk**, and **Agents without owners** summary cards.
1. [] Review the **Status**, **Publisher type**, **Platform**, **Channel**, **Tags**, and **Usage** filters.
1. [] Review the **Copilot Studio**, **Microsoft**, **Third party**, and **Others** platform groups.
1. [] Confirm that the map provides a visual path from the organization-wide estate to agents that require investigation.
1. [] In the map, select the **Copilot Studio** platform group to display its agents.
1. [] Locate and select **LAB682 Supplier Risk Agent**.
1. [] In the agent summary card, review **Users**, **Sessions**, and **Exceptions**, and then select **View details**.
1. [] Select the **Users** tab, and review **Installed for**, **Available to**, and **Shared with** to see where the agent is installed and who can access it.
1. [] On the **Details** tab, review **About this agent** and the displayed version.
1. [] Note that the description in **About this agent** was provided by the agent creator.
1. [] Expand **Instructions**.
1. [] Review the instruction text that guides how the agent operates at run time.
1. [] Expand **Environment**, and then review **Environment name**, **Environment type**, and **Environment ID**.
1. [] Expand **Properties**, and then review **Editors**, **Agent location**, and **Model name**.
1. [] Confirm that **Model name** is **GPT5Chat**.
1. [] Scroll to the top of the details pane, and then select **Activity**.
1. [] Under **Usage**, review **Active users**, **Sessions**, **Exceptions**, and **Agent run-time** for the **Past 30 days**.
1. [] Scroll down, and then review **Active users over time** and **Sessions over time** for this agent.

> [!Knowledge]
> Agent Map complements the detailed Agent Registry by visualizing adoption across platforms and highlighting populations that may require governance action.

> [!Help]
> If **Agents at risk** displays **Failed to load** but the map reports **All agents loaded successfully**, continue with the map review. If the map itself does not load, notify the lab proctor and stop.

===

# Locate your assigned agent

In this task, you will use the Agent Registry to locate the unique Agent ID assigned to you.

1. [] On the **All agents** page, select **Registry**.
2. [] Review the unified inventory of agents from different platforms and publishers.
3. [] In **Search for an Agent**, enter: 

+++@lab.CloudCredential(Ignite2026-Lab682).LCW_AgentName+++.
4. [] In the search results, locate the agent named **@lab.CloudCredential(Ignite2026-Lab682).LCW_AgentName**.
5. [] Confirm that only the exact assigned display name is visible, and then select the agent.

> [!Alert]
> This tenant is shared with other attendees. Use only the agent name assigned to you, and do not modify any agent or permission.

===

# Investigate the assigned agent

In this task, you will inspect the assigned agent's identity, origin, capabilities, permissions, and activity.

1. [] In the agent details pane, verify that the display name is **@lab.CloudCredential(Ignite2026-Lab682).LCW_AgentName**.
1. [] On the **Details** tab, review the agent's description, status, publisher, publisher type, channels, and platform.
1. [] Locate **Owner**, and verify that the signed-in lab account is assigned as the owner.
1. [] Select **Permissions**, and then review the permission information available in Microsoft Agent 365.
1. [] Under **Weather API**, observe that the **Weather.Read** claim has the **Delegated** permission type.

> [!Note]
> The delegated **Weather.Read** permission enables an on-behalf-of (OBO) flow so the agent can read weather data on behalf of the signed-in user.

6. [] Select **Security**.
7. [] Under **Microsoft Entra protections**, locate **Review in Entra**.
8. [] Select **Review in Entra** to examine the Agent ID, its blueprint, and their permissions in greater detail.

> [!Knowledge]
> Agent 365 provides an organization-wide inventory and operational view. Microsoft Entra provides the identity and authorization view for the Agent ID and the blueprint from which it was created.

===

# Review the Agent ID in Entra

In this task, you will follow the Agent 365 security handoff into Microsoft Entra and review the assigned Agent ID, its blueprint, and their permissions.

> [!Alert]
> Review only **@lab.CloudCredential(Ignite2026-Lab682).LCW_AgentName** and **Ignite Weather Blueprint**. Do not make any changes such as to owners, sponsors, permissions, credentials, or status.

1. [] If prompted, sign in to the Microsoft Entra admin center with your lab-assigned credentials. 

Username: +++@lab.CloudCredential(Ignite2026-Lab682).EntraUsername+++ 

TAP: +++@lab.CloudCredential(Ignite2026-Lab682).TAP+++

2. [] Confirm that the Microsoft Entra admin center opens **@lab.CloudCredential(Ignite2026-Lab682).LCW_AgentName | Overview**.
4. [] In the agent identity details card, verify that the object is labeled **Agent identity** and its **Status** is **Active**.
5. [] Review the **Sponsors** and **Owners** assigned to the Agent ID.
6. [] Under **Agent blueprint**, verify that the linked blueprint is **Ignite Weather Blueprint**.
7. [] Review the **Blueprint App ID**, **Object ID**, **Application ID**, and **Created on** values, and verify that **Object ID** matches ++@lab.CloudCredential(Ignite2026-Lab682).LCW_AgentID++.
8. [] In the **Agent identity's access** card, review the **Permissions** and **Entra roles** counts.
9. [] In the left navigation menu, under **Access**, select **Granted permissions**.
10. [] On the **Permissions** tab, under **Admin consent**, locate **Weather API**.
11. [] Verify that **Weather.Read** has the **Delegated** permission type and is inherited from the agent blueprint.
12. [] Confirm that the Weather API permission information aligns with the delegated **Weather.Read** permission previously reviewed in Microsoft Agent 365.
13. [] In the left navigation menu, select **Overview**.
14. [] Under **Agent blueprint**, select **Ignite Weather Blueprint**.

> [!Help]
> If Microsoft 365 admin center reports that your account does not have permission, sign out and sign in again once. If the message remains, notify the lab proctor and stop. Do not use another account or open a different agent.

> [!Knowledge]
> A blueprint provides a consistent identity and delegated-authorization foundation for related Agent IDs. Reviewing both levels helps you identify which permissions are inherited and which permissions apply only to one Agent ID.

===

# Review the agent blueprint

In this task, you will review the blueprint that provides the identity and delegated authorization foundation for the lab's agents.

1. [] On the blueprint details page, select **Granted permissions**.
1. [] Verify that **Weather API** includes the delegated permission **Weather.Read**.
1. [] Under **Linked agent identities**, locate the agent identity named **@lab.CloudCredential(Ignite2026-Lab682).LCW_AgentName**.
1. [] Verify that **@lab.CloudCredential(Ignite2026-Lab682).LCW_AgentName** is associated with **@lab.Variable(LCW_blueprintName)**.
1. [] Open the application details for **@lab.CloudCredential(Ignite2026-Lab682).LCW_AgentName**.
1. [] Select **Granted permissions** to review the permissions assigned to this agent identity.
1. [] Verify that **Weather.Read** is shown as inherited from **@lab.Variable(LCW_blueprintName)** as **Delegated** type.

> [!Knowledge]
> A blueprint provides a consistent identity and governance foundation for agents created from it. Blueprint-level configuration can reduce per-agent administration while preserving separate identities for monitoring and governance. The blueprint in this lab is configured so linked agent identities inherit delegated permissions.

===

# Complete Section 1

You used Microsoft Agent 365 and Microsoft Entra to:

- Assess organization-wide agent inventory and governance insights.
- Visualize agent sprawl across platforms and publishers.
- Locate and inspect your assigned Agent ID.
- Review its linked blueprint and inherited **Weather.Read** permission.

> [!Note]
> You completed Section 1. Continue to **Section 2: Govern agent identities and lifecycle** to test delegated access, request autonomous access, and manage agent sponsorship.

===

# Section 2: Govern agent identities and lifecycle

**Section 2 of 3 | Estimated time: 20 minutes**

Governance includes controlling what an agent can access and maintaining accountable human ownership throughout the agent's lifecycle. This section contains two related governance sessions that were validated as separate learner journeys.

- **Session 2A** governs delegated and autonomous permissions.
- **Session 2B** governs sponsorship changes through lifecycle automation.

> [!Note]
> The two sessions use separate prepared scenarios. Follow the agent and identity names specified in each session rather than assuming that every resource is shared.

===

# Session 2A: Govern agent permissions

AI agents need controlled, intentional, and auditable access to resources. Building on the blueprint and inherited permission review from Section 1, you will validate delegated access, observe an autonomous request being denied, and grant the missing application permission through an access package.

In this session, you will:

- Validate inherited delegated access through an on-behalf-of request.
- Observe least-privilege enforcement for an autonomous request.
- Request missing application access through an access package.
- Verify that the new permission changes the authorization result.

===

# Validate delegated agent access

In this task, you will confirm that a delegated agent can call the Weather API on behalf of the signed-in user.

1. [] Open the [Weather Agent Service](https://aka.ms/lab682).
1. [] Verify that the agent identity named **@lab.CloudCredential(Ignite2026-Lab682).LCW_AgentName** is selected by default in the drop down menu.
1. [] Verify the **On-Behalf-Of** flow is selected.
1. [] In the prompt field, enter ++What's the weather in Dallas?++.
1. [] Select **Send**.
1. [] Verify that the agent returns weather information for Dallas without an authorization error.

> [!Knowledge]
> In an on-behalf-of flow, the agent uses delegated authorization associated with the signed-in user. The result demonstrates that the blueprint and delegated permission are configured for this scenario.

7. [] Select **View Agent ID Flow Diagram** and review how the user, agent identity, identity platform, sidecar, and Weather API participate in the token flow.

===

# Observe denied autonomous access

In this task, you will observe how the environment responds when an autonomous agent lacks an application permission.

1. [] Open the [agent permission view]("@lab.CloudCredential(Ignite2026-Lab682).LCW_AgentNamePermissionURL").
1. [] Verify that the **Weather.Read.All** application permission for **Weather API** is not assigned.
1. [] Return to the [Weather API service](https://aka.ms/lab682).
1. [] Select the **Autonomous** flow.
1. [] In the prompt field, enter ++What's the weather in Los Angeles?++.
1. [] Select **Send**.
1. [] Verify that the request fails with a *403 Forbidden* authorization response.
1. [] Verify that the response provides a [**request access here**](https://myaccess.microsoft.com/@ignite26.onmicrosoft.com#/access-packages/2d9472cd-5278-4ee1-a63d-c10c4f260afa) link.

> [!Knowledge]
> Autonomous agents use application authorization rather than a signed-in user's delegated authorization. The denied request demonstrates least privilege: the agent cannot access the API until the required permission is explicitly granted.

===

# Request autonomous agent access

In this task, you will request the missing application permission through a governed access package.

1. [] In the denied request, select [**request access here**](https://myaccess.microsoft.com/@ignite26.onmicrosoft.com#/access-packages/2d9472cd-5278-4ee1-a63d-c10c4f260afa).
1. [] On the access request page, verify that the selected package is **Weather API Access (App Permission)**.

    > [!Help]
    > The page may display a "no valid policy was found in the request" message. If the request options are available, continue with the next step.

1. [] Select **Others**, and then select **Sponsored agent**.

    > [!Help]
    > If **Others** is unavailable, wait 30 seconds before proceeding.

1. [] Under **Sponsored agents**, select **@lab.CloudCredential(Ignite2026-Lab682).LCW_AgentName**.
1. [] Verify that the request applies to **@lab.CloudCredential(Ignite2026-Lab682).LCW_AgentName**.
1. [] Select **Continue**.
1. [] In the business justification field, enter +++Checking out the Ignite Lab682!+++.
1. [] Review the requested access, and then select **Submit request**.

    > [!Note]
    > The access package in this lab is configured for automatic approval, so you do not need to complete a separate approval action.

1. [] Verify that the access request is submitted, and then open the [agent permission view]("@lab.CloudCredential(Ignite2026-Lab682).LCW_AgentNamePermissionURL").
1. [] Wait 30 seconds, and then select **Refresh**.
1. [] If **Weather.Read.All** is not displayed, continue refreshing every 30 seconds for up to 3 minutes.

    > [!Help]
    > If **Weather.Read.All** does not appear after 3 minutes, notify the lab proctor before continuing.

1. [] Confirm that the **Application** permission **Weather.Read.All** is assigned, and then return to the [Weather API service](https://aka.ms/lab682).

> [!Alert]
> If the **request access here** link does not open the access package page, [open the access package directly](@lab.Variable(LCW_accessPackageURL)).

===

# Validate autonomous agent access

In this task, you will retry the original request and verify that the governed permission changes the authorization result.

1. [] Return to the [Weather API service](https://aka.ms/lab682).
1. [] Refresh the Weather API service by pressing `F5`.
1. [] Select the **Autonomous** flow.
1. [] In the prompt field, enter +++What's the weather in Los Angeles?+++.
1. [] Select **Send**.
1. [] Verify that the agent returns weather information for Los Angeles.
1. [] Select **View Agent ID Flow Diagram** and identify the application authorization used for the successful request.

> [!Knowledge]
> The same agent request changed from denied to allowed only after the governed access process granted the required application permission.

===

# Complete Session 2A

You used Microsoft Entra Agent ID governance capabilities to:

- Validate blueprint-based delegated authorization through an on-behalf-of request.
- Observe least-privilege enforcement for an autonomous agent.
- Request and verify application access through a governed access package.

> [!Note]
> You completed the permission-governance session. Continue to **Session 2B: Govern agent lifecycle** to manage what happens when an agent's sponsor changes roles or leaves the organization.

===

# Session 2B: Govern agent lifecycle

Every agent identity needs an accountable human sponsor throughout its lifecycle. Organizations use mover and leaver workflows when an employee changes roles or leaves the organization. In this session, you will run a lifecycle workflow that prevents orphaned agents by transferring sponsorship to the current sponsor's manager and sending a notification about the change.

In this session, you will:

- Review the current accountable sponsor.
- Run an on-demand lifecycle workflow.
- Verify that sponsorship transfers to the current sponsor's manager.
- Confirm that the new sponsor receives a notification.

> [!Note]
> Session 2B uses a separate preconfigured agent and sponsor scenario. You'll be using the Agent ID **@lab.CloudCredential(Ignite2026-Lab682).GovernedAgentName** rather than the Agent ID from Session 2A.

===

# Review the current sponsor

In this task, you will verify the accountable sponsor before running the lifecycle workflow.

1. [] In a new browser tab, open the [Microsoft Entra admin center](https://entra.microsoft.com), and then sign in. 

Username: +++@lab.CloudCredential(Ignite2026-Lab682).EntraUsername+++ 

TAP: +++@lab.CloudCredential(Ignite2026-Lab682).TAP+++

2. [] In the left pane, scroll up if needed, select **Agents**, and then select **Agent identities**.
4. [] In **Search by agent name or object ID**, enter **@lab.CloudCredential(Ignite2026-Lab682).GovernedAgentName**, and then select the matching agent.
5. [] Under **Access**, select **Owners and sponsors**.
6. [] On **Owners and sponsors**, verify that **@lab.CloudCredential(Ignite2026-Lab682).currentSponsorName** appears with a Type value of **Sponsor**.

> [!Knowledge]
> A sponsor is the person accountable for an agent's ongoing business need and access. Lifecycle automation helps preserve that accountability when organizational responsibilities change.

> [!Note]
> In this lab scenario, Microsoft Entra ID lists your account as the **Manager** of **@lab.CloudCredential(Ignite2026-Lab682).currentSponsorName**. The lifecycle workflow will read this directory relationship to identify you as the new sponsor.

===

# Transfer agent sponsorship

In this task, you will run an on-demand lifecycle workflow on your account's reportee which will notify the manager (you) and will then transfer sponsorship to the manager (you).

1. [] If the Microsoft Entra admin center is already open, select **Home**; otherwise, open the [Microsoft Entra admin center](https://entra.microsoft.com) and sign in. 

Username: +++@lab.CloudCredential(Ignite2026-Lab682).EntraUsername+++ 

TAP: +++@lab.CloudCredential(Ignite2026-Lab682).TAP+++

2. [ ] In the left pane, scroll down if needed, expand **ID Governance**, and select **Lifecycle workflows**.

    > [!Help]
    > If a **You do not have access** page appears, stop and notify the lab proctor. Do not select or run a different workflow.

1. [] On **Lifecycle workflows | Overview**, under **Workflows**, select **Workflows**.
1. [] Open the workflow named **Offboard Agent Sponsors (Real-time)**.
1. [] Select **Tasks**.
1. [] Verify that the workflow includes the **Send email to manager about sponsorship changes** task.
1. [] Verify that the workflow includes the **Transfer agent sponsorships to manager** task.
1. [] Select **Overview**.
1. [] Select **Run on demand**.
1. [] On the **Offboard Agent Sponsors (Real-time) - run on demand** page, select **+ Select users**.
1. [] In **Search for users**, enter +++**@lab.CloudCredential(Ignite2026-Lab682).currentSponsorName**+++, and then select the matching user as the workflow subject.
1. [] Select **Select** to add the workflow subject.
1. [] Select **Run workflow** to start the workflow.
1. [] When the workflow-started notification appears, wait for the **Workflow history** page to open.
1. [] If no workflow run is listed initially, select **Refresh**.
1. [] Continue selecting **Refresh** until the workflow status is **Completed**.
1. [] In the workflow history, select **@lab.CloudCredential(Ignite2026-Lab682).currentSponsorName**.
1. [] In the **Tasks** pane, verify that **Send email to manager about sponsorship changes** has a status of **Completed**.
1. [] Verify that **Transfer agent sponsorships to manager** has a status of **Completed**.

    > [!Help]
    > If either required task has a status of **Failed** or **Canceled**, stop and notify the lab proctor. A workflow run can display **Completed** even when an individual task did not complete.

    > [!Note]
    > The on-demand run represents tasks that an organization could trigger automatically as part of a mover or leaver process.

1. [] In the left pane, scroll up if needed. Under **Entra ID** select **Agents**, and then select **Agent identities**.
1. [] In **Search by agent name or object ID**, enter **@lab.CloudCredential(Ignite2026-Lab682).GovernedAgentName**, and then select the matching agent.
1. [] Under **Access**, select **Owners and sponsors**.
1. [] On **Owners and sponsors**, verify that **@lab.CloudCredential(Ignite2026-Lab682).EntraUsername** appears with a Type value of **Sponsor** and that **@lab.CloudCredential(Ignite2026-Lab682).currentSponsorName** is no longer listed as a sponsor.

> [!Knowledge]
> The **Transfer agent sponsorships to manager** task looks up the manager attribute of the current sponsor in Microsoft Entra ID.

===

# Verify the sponsor notification

In this task, you will verify that the new sponsor received a notification containing the transferred agent identity.

1. [] In a new browser tab, open [Outlook on the web](https://outlook.office.com/mail/).
2. [] Sign in if needed. 

Username: +++@lab.CloudCredential(Ignite2026-Lab682).EntraUsername+++ 

TAP: +++@lab.CloudCredential(Ignite2026-Lab682).TAP+++

3. [] In the inbox, locate the most recent message with the subject **Agent Sponsor's job profile changed or sponsor has left the organization**.
5. [] Open the message, and then verify that it is addressed to **@lab.CloudCredential(Ignite2026-Lab682).EntraUsername**.
6. [] Verify that the sender is **Microsoft Azure on behalf of Ignite 2026** and that the sender address is **lifecycleworkflows-noreply@microsoft.com**.
7. [] Verify that the message identifies **@lab.CloudCredential(Ignite2026-Lab682).currentSponsorName** as a sponsor who moved or left the organization.
8. [] Under the list of agents sponsored by **@lab.CloudCredential(Ignite2026-Lab682).currentSponsorName**, locate **@lab.CloudCredential(Ignite2026-Lab682).GovernedAgentName**.
9. [] Verify that the listed agent has an object ID and that its object type is **Agent Identity**.

> [!Note]
> The displayed recipient uses the lab attendee's display name rather than the username.

===

# Complete Section 2

You used Microsoft Entra ID Governance capabilities to:

- Test delegated and autonomous agent authorization.
- Request missing agent access through an access package.
- Verify the accountable sponsor for an agent identity.
- Run a lifecycle workflow that transferred sponsorship.
- Confirm the resulting sponsor notification.

> [!Note]
> You completed Section 2. Continue to **Section 3: Protect agent access to resources** to explore how Global Secure Access can enforce network protection for an AI agent.

===

# Section 3: Protect agent access to web resources

**Section 3 of 3 | Estimated time: 20 minutes**

AI agents can access websites, upload organizational data, and connect to external tools via remote Model Context Protocol (MCP) servers. In this section, you will use Global Secure Access controls to distinguish user activity from local agent activity and enforce network protections based on the initiating session.

In this section, you will:

- Join the lab device to Microsoft Entra ID and verify that the Global Secure Access client is connected.
- Review the web content filtering, content, and MCP policies that apply to agent sessions.
- Compare user-initiated activity with the same activity initiated by a local agent.
- Verify that Global Secure Access blocks restricted agent web access, file uploads, and MCP connections.
- Locate blocked agent transactions in the Global Secure Access traffic logs.

> [!Note]
> The activities in this section use GitHub Copilot CLI as a local agent. Global Secure Access identifies the initiating process and agentic session so that policies can apply to agent activity without blocking the equivalent user activity.

===

# View Network Control Policies

In this task, you will review the preconfigured Global Secure Access policies for web destinations, file uploads, and MCP servers, and then confirm that they are linked to the baseline security profile.

1. [] Open a browser and navigate to entra.microsoft.com.
2. [] Navigate to Global Secure Access > Secure > Web content filtering policies
3. [] Select **Go to Web Filtering Policy (v2)** in the blue banner.
4. [] Select the policy **Block Agent from Restricted Web Content**.
5. [] Select **Rules**.
6. [] Select the rule **Restricted Liability Web Categories**.
7. [] Note the following configuration:
    - Session type: **Agent**
    - Web categories: Includes **Gambling**
    - Rule action: **Block**
8. [] Next, navigate to Global Secure Access > Secure > Content policies
9. [] Select the policy **Block Agent File Uploads**.
10. [] Select **Rules**.
11. [] Select the rule **Block Agent Uploads** by selecting the edit button.
12. [] Note the following configuration:
    - Action: **Block**
    - Activities: **Upload**
    - Session type: **Agent**
13. [] Next, navigate to Global Secure Access > Secure > MCP policies
14. [] Select the policy **Unauthorized MCP for Agents**
15. [] Select **Rules**.
16. [] Select the rule **Restricted MCP Servers**.
17. [] Note the following configuration:
    - MCP matching conditions lists **mcp.deepwiki.com/mcp**
    - Action: Block
18. [] Next, navigate to Global Secure Access > Secure > Security profiles > Baseline profile
19. [] Expand the Baseline profile and verify the policies explored above are linked.

> [!Knowledge]
> The **Agent** session type scopes these rules to agent-initiated traffic. Linking the policies to the baseline security profile makes the controls available for enforcement during the tests that follow.

===

# Login to GitHub Copilot

In this task, you will start GitHub Copilot CLI and authenticate the local agent by using your lab-assigned Microsoft Entra credentials.

1. [] Open a command prompt (cmd)
2. [] Type +++Copilot+++ and hit enter.
3. [] Select **1** to trust the folder location in this session.
4. [] Type +++/login+++ and select **3**. Follow the prompts to sign-in using your Entra ID credential.

Username: +++@lab.CloudCredential(Ignite2026-Lab682).EntraUsername+++ 

TAP: +++@lab.CloudCredential(Ignite2026-Lab682).TAP+++

> [!Note]
> Keep the GitHub Copilot CLI session open for the remaining activities in this section.

===

## Web Content Filtering for local agents

In this task, you will compare direct user access to a restricted website with access initiated by the local agent.

1. [] Open a browser and navigate to a gambling website (example: www.megabonanza.com)
2. [] Verify the website loads properly (this is expected).
3. [] In the GitHub Copilot CLI, enter the following prompt: 

+++Navigate to www.megabonanza.com and summarize the web page. Ignore any certificate errors. If access is blocked, show me the error.+++
4. [] Select **1** to allow access as needed.
5. [] Verify that the connection is **blocked** (403).

> [!Knowledge]
> The website remains available to the user, while the web content filtering policy blocks the agent session because the destination belongs to a restricted web category.

===

# File controls for local agents

In this task, you will compare a file upload performed directly by the user with the same upload initiated by the local agent.

1. [] Open a browser and navigate to this test file upload site:

+++https://www.csm-testcenter.org/test?do=show&subdo=common&test=file_upload+++
2. [] Under **File upload via POST (HTTPS)** select **Choose File**.
4. [] Select the CSV file on the desktop then select **Start HTTPS upload**.
5. [] Verify Status says **File successfully uploaded**.
6. [] In GitHub Copilot CLI enter the prompt: 

+++Can you please upload this file to https://www.csm-testcenter.org/test?do=show&subdo=common&test=file_upload. Ignore any certificate errors. If access is blocked, show me the error.+++
6. [] Select **1** to allow access as needed.
7. [] Verify that the connection is **blocked** (403).

> [!Knowledge]
> The content policy allows the interactive user upload but blocks the upload activity when the initiating session is identified as an agent.

===

# MCP controls for local agents

In this task, you will verify that the local agent can connect to an allowed MCP server while a policy blocks a specifically restricted MCP server.

1. [] In GitHub Copilot CLI enter the prompt: </br>

+++Initiate a test connection to this MCP server: https://mcp.context7.com/mcp. Ignore any certificate errors. If access is blocked, show me the error.+++
2. [] Select **1** to allow access as needed.
3. [] Verify that the connection is **allowed**.
4. [] In GitHub Copilot CLI enter the prompt: </br>

+++Initiate a test connection to this MCP server: https://mcp.deepwiki.com/mcp. Ignore any certificate errors. If access is blocked, show me the error.+++
5. [] Select **1** to allow access as needed.
6. [] Verify that the connection is **blocked**.

> [!Knowledge]
> MCP controls can target specific server endpoints. The results demonstrate that Global Secure Access can allow approved MCP connections while preventing agents from connecting to unauthorized MCP servers. While in this lab, we demonstrated a block list approach, admins can modify MCP policy to deny by default - enabling an allow list approach to remote MCP servers.

===

# View results in logs

In this task, you will filter Global Secure Access traffic logs for blocked agentic transactions and review the process and policy details associated with the enforcement decisions.

1. [] Navigate to entra.microsoft.com > Global Secure Access > Monitor > Traffic logs
2. [] Select the **Transactions** tab.
3. [] Add a filter **Is Agentic** set to **Yes** and click **Apply**. 
4. [] Add a 2nd filter **Action** set to **Block** and click **Apply**.
5. [] Select one or more logs and review details such as Initiating process name and policy information.

> [!Knowledge]
> The **Is Agentic** value distinguishes agent traffic from user traffic. Transaction details provide the evidence needed to correlate a blocked request with its initiating process and the policy that enforced the decision.

===

# Complete Section 3

You used Global Secure Access capabilities to:

- Verify that the lab device and Global Secure Access client were ready for policy evaluation.
- Review web content filtering, content, and MCP controls scoped to agent sessions.
- Compare direct user activity with local agent activity.
- Confirm that restricted agent web access, file uploads, and MCP connections were blocked.
- Correlate blocked agent activity with Global Secure Access traffic logs.

## Learn more

- [Agent overview in the Microsoft 365 admin center](https://learn.microsoft.com/en-us/microsoft-365/admin/manage/agent-365-overview?view=o365-worldwide "Microsoft Learn: Agent overview")
- [Use Agent Map in the Microsoft 365 admin center](https://learn.microsoft.com/en-us/microsoft-365/admin/manage/agent-map?view=o365-worldwide "Microsoft Learn: Agent Map")
- [Manage agent identities in Microsoft Entra](https://learn.microsoft.com/en-us/entra/agent-id/manage-agent-identities-admin "Microsoft Learn: Manage agent identities")
- [Authorization in Microsoft Entra Agent ID](https://learn.microsoft.com/en-us/entra/agent-id/authorization-agent-id "Microsoft Learn: Authorization in Agent ID")
- [Govern agent identities with Microsoft Entra ID Governance](https://learn.microsoft.com/en-us/entra/id-governance/agent-id-governance-overview "Microsoft Learn: Agent ID governance overview")
- [Use access packages for agent identities](https://learn.microsoft.com/en-us/entra/agent-id/agent-access-packages "Microsoft Learn: Access packages for agent identities")
- [Use agent sponsor tasks in Lifecycle Workflows](https://learn.microsoft.com/en-us/entra/id-governance/agent-sponsor-tasks "Microsoft Learn: Agent sponsor tasks")
- [Global Secure Access for Copilot Studio agents](https://learn.microsoft.com/en-us/power-platform/admin/security/secure-web-ai-gateway-agents "Microsoft Learn: Global Secure Access for Copilot Studio agents")