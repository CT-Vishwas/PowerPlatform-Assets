# Hands-On Lab 2: Agent Actions & Ticket Topic <span class="badge-lab">LAB 2</span>

> **Lab tile**  
> **Scenario:** Let employees check an asset's status and start an IT ticket through a conversation with the helpdesk agent.  
> **Time:** 55 minutes  
> **Environment:** Development environment containing the `IT Asset and Helpdesk Hub` solution

## Objectives
- Add the asset lookup flow to the IT Helpdesk agent as an action.
- Create a topic that gathers ticket details and creates a Dataverse `IT Ticket` row.
- Test the asset lookup and ticket creation conversations and verify the resulting data.

## Steps

### Step 1: Confirm the Agent and Flow Are Ready
1. Open [Copilot Studio](https://copilotstudio.microsoft.com) and select the Development environment.
2. Open **IT Helpdesk & Asset Copilot**.
3. Confirm that the flow from Lab 28, `Agent Action - Lookup Hardware Asset`, is saved and turned on in the `IT Asset and Helpdesk Hub` solution.
4. Confirm the flow accepts the text input `AssetTag` and returns the text output `AssetStatusSummary`. Use the names configured in your flow if they differ.
5. If the flow is missing, complete Lab 28 before continuing.

### Step 2: Add the Asset Lookup Flow as an Agent Action
1. In the agent, open **Tools** or **Actions** and select **+ Add an action**.
2. Choose the existing `Agent Action - Lookup Hardware Asset` flow. Do not create a second flow for this lab.
3. Review the action's input and output:
   - **Input:** `AssetTag` — the hardware asset tag, such as `LAP-101`.
   - **Output:** `AssetStatusSummary` — the response returned by the flow.
4. If the studio offers dynamic or AI input filling, enable it for `AssetTag` and confirm its description explains that an asset tag such as `LAP-101` is expected.
5. Add the action to the agent and save. If prompted to create or select a connection, use the authorized training connection.

### Step 3: Create the Ticket Topic
1. Open **Topics** and select **+ Add a topic** → **From blank**.
2. Name the topic **Log IT Ticket**.
3. Add these trigger phrases, one per entry:
   - `My laptop is broken`
   - `Log an IT ticket`
   - `Report a hardware issue`
4. Save the topic so the phrases can be used to route a test conversation to it.

### Step 4: Collect the Ticket Details
1. In the topic authoring canvas, add a **Question** node after the trigger.
2. Ask: `What is the asset tag?` Set the response to a text variable named `varAssetTag`.
3. Add another **Question** node: `Briefly describe the issue. Include the asset tag in the description if it is relevant to the problem.` Set the response to a text variable named `varDescription`.
4. Add a **Question** node for priority. Ask: `What priority should this ticket have?` Offer **Low**, **Medium**, **High**, and **Critical**, and save the response as `varPriority`.
5. Review the variables in the canvas and confirm each question stores its answer in the intended variable.

### Step 5: Create the Dataverse Ticket
1. After the questions, add an action node and select **Microsoft Dataverse** → **Add a new row**. In some studio layouts this may appear as **Call an action**.
2. Select the `IT Ticket` table. If it is not available, confirm the table is in the selected environment and that the agent's Dataverse connection has permission to create rows.
3. Map the fields:
   - **Ticket Title:** `varDescription` (or a short title collected in an additional question if your table requires one).
   - **Description:** `varDescription`.
   - **Priority:** `varPriority`. Match the choices to the labels configured for the table.
   - **Ticket Status:** choose **Open** if the column is required or available.
4. If **Related Asset** is optional, leave it blank for this exercise and retain the asset tag in the description. A text value such as `LAP-101` is not itself a Dataverse lookup reference; only populate the lookup if you select a valid asset row or have configured a separate step to resolve the tag to that row.
5. Add a confirmation message after the Dataverse action, such as: `Your IT ticket has been submitted.` If the action exposes an error branch, provide a clear message that ticket creation failed and the user should contact the IT Helpdesk.
6. Save the topic and the agent.

### Step 6: Test Both Agent Capabilities
1. Open **Test your agent** and start a new conversation.
2. Test the asset lookup by asking: `What is the status of LAP-101?` Confirm the agent invokes the lookup action and returns the flow's asset summary.
3. Start a separate test conversation with: `Log a high priority ticket for LAP-101`.
4. Answer the topic's questions with a description, for example `Laptop will not power on`, and the priority **High**.
5. Confirm the agent reports success only after the Dataverse action completes.
6. In the `IT Asset and Helpdesk Hub` solution, open the `IT Ticket` table's data grid and verify that a row was created with the description and priority you supplied.
7. If the topic does not trigger, check its trigger phrases and save the topic. If ticket creation fails, review the Dataverse action's required fields, choice mappings, and connection permissions.

**Time Allocation:** 55 Minutes
