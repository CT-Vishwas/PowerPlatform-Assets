# Hands-On Lab 3: Publish Agent & Portal Preview <span class="badge-lab">LAB 3</span>

> **Lab tile**  
> **Scenario:** Publish the IT Helpdesk agent, test it through a channel, and inspect how a Power Pages site controls access to Dataverse data.  
> **Time:** 40 minutes  
> **Environment:** Development environment containing the `IT Asset and Helpdesk Hub` solution

## Objectives
- Publish the completed IT Helpdesk agent and verify it through a channel outside the authoring canvas.
- Explore Power Pages site areas and inspect how table permissions and web roles control access to Dataverse tables.
- Verify that the lab components are organized in the `IT Asset and Helpdesk Hub` solution.

## Steps

### Step 1: Prepare the Agent for Publishing
1. Open [Copilot Studio](https://copilotstudio.microsoft.com) and select the Development environment.
2. Open **IT Helpdesk & Asset Copilot**.
3. Before publishing, confirm that the agent's instructions and approved IT policy knowledge source are present.
4. Open **Test your agent** and ask a policy question that can be answered from the knowledge source. Confirm the response is relevant and does not invent policy details.
5. Save any pending changes. Resolve any validation messages shown by Copilot Studio before continuing.

### Step 2: Publish the Agent
1. Select **Publish** in Copilot Studio.
2. Review the confirmation prompt and select **Publish** again if requested.
3. Wait for the publish operation to finish and confirm the studio reports that the agent is published.
4. If publishing fails, read the reported issue, address it, and retry. Do not continue to channel testing until publishing succeeds.

### Step 3: Test a Published Channel
1. Open the agent's **Channels** page. Depending on the studio layout, you may need to open **Settings** or the channel list from the agent navigation.
2. Select the **Demo Website** or **Website** channel and open its demo URL. If the channel needs to be configured first, follow the studio prompts and then return to the channel details.
3. In the browser chat, ask: `What should I do if my company laptop stops working?`
4. Confirm the published agent responds using the IT policy knowledge source. If citations or source links are displayed, verify they refer to the expected source.
5. Ask a question that is not answered by the source, such as: `Can I keep my laptop after I leave the company?` Confirm the agent does not guess and directs the employee to the IT Helpdesk.
6. If the published agent does not reflect your latest changes, verify the agent was successfully published after those changes and publish again if needed.
7. **Optional:** If Microsoft Teams is enabled for your training environment, inspect its channel setup and test the agent there only if your instructor has provided access. Channel availability and setup requirements can vary by tenant.

### Step 4: Explore the Power Pages Site
1. Open [Power Pages](https://make.powerpages.microsoft.com) and select the Development environment.
2. Open the training site if one has been provided. If no site is available, inspect the Power Pages Design Studio without creating or publishing a site.
3. Review the main design areas:
   - **Pages:** inspect the site's page structure.
   - **Styling:** review the visual theme and page styling.
   - **Data:** inspect the Dataverse tables available to the site.
   - **Set up:** review site configuration and security-related options.
4. Locate the area's table-permission settings. Depending on the current studio layout, these may be in **Set up** or the site's security settings.
5. Inspect, but do not change, permissions for the `IT Asset` and `IT Ticket` tables. Note which access types are configured, such as **Read**, **Write**, or **Create**, and which web roles (for example, **Authenticated Users**) they apply to.
6. Discuss what those permissions would allow a signed-in site user to do. Do not grant **Write** or **Create**, change a web role, or publish security changes unless your instructor explicitly asks you to.

### Step 5: Verify the Solution
1. Return to [Power Apps](https://make.powerapps.com), select the Development environment, and open **Solutions**.
2. Open **IT Asset and Helpdesk Hub**.
3. Confirm the **IT Helpdesk & Asset Copilot** agent is listed in the solution alongside the lab components created so far, such as tables, apps, and flows.
4. If you created the agent outside the solution and it is missing, use **Add existing** → **Agent** to add it. Avoid adding unrelated components or making changes to the Default Solution.

**Time Allocation:** 40 Minutes
