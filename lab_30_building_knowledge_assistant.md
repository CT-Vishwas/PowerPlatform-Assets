# Hands-On Lab 1: Create Agent & Add Knowledge <span class="badge-lab">LAB 1</span>

> **Lab tile**  
> **Scenario:** Build an IT Helpdesk assistant that answers employee questions using approved IT policies.  
> **Time:** 40 minutes  
> **Environment:** Development environment containing the `IT Asset and Helpdesk Hub` solution

## Objectives
- Create an IT Helpdesk agent in the development environment and add it to the solution.
- Configure the agent to answer IT policy questions using an approved knowledge source.
- Test that answers are relevant to the source and that unsupported questions are handled appropriately.

## Steps

### Step 1: Open the Development Solution
1. Open [Power Apps](https://make.powerapps.com) and sign in with your training account.
2. Select your **Development** environment using the environment selector in the top navigation.
3. In the left navigation, select **Solutions**.
4. Open **IT Asset and Helpdesk Hub**. Confirm that you are in the intended solution before creating the agent.

### Step 2: Create the Agent
1. In the solution, select **+ New** → **Agent**. If the agent option is not available in your portal, open [Copilot Studio](https://copilotstudio.microsoft.com), select the same Development environment, and create the agent there.
2. Enter **IT Helpdesk & Asset Copilot** as the agent name.
3. If prompted for a description, enter: `Answers employee IT questions using approved IT policies and helpdesk guidance.`
4. Create the agent and wait for its overview or authoring page to open.
5. If you created the agent from Copilot Studio rather than the solution, return to **IT Asset and Helpdesk Hub** and add the existing agent to the solution using **Add existing** → **Agent**. This keeps the agent with the rest of the lab components for solution management.

### Step 3: Add Agent Instructions
1. Open the agent's **Instructions** field. Depending on the studio layout, this may be on the overview page or under the agent's settings.
2. Enter these instructions:

   > You are an enterprise IT Helpdesk assistant. Answer employee questions about IT policies and hardware using the knowledge sources configured for this agent. Be concise, use plain language, and do not invent policy details. If the available sources do not answer a question, say that you could not verify the answer and direct the employee to the IT Helpdesk.

3. Save the changes if the studio does not save automatically.

### Step 4: Add an IT Policy Knowledge Source
1. Open the agent's **Knowledge** section and select **+ Add knowledge**.
2. Choose a source available in your training environment:
   - **File:** Upload the instructor-provided `IT_Hardware_Policy.pdf` or another approved IT policy document.
   - **Website:** Enter the URL of an approved IT FAQ or policy page that the agent can access.
3. Follow the prompts to add the source. If asked for a name or description, use `IT Hardware Policy` and `Approved guidance on employee hardware and support policies`.
4. Wait for the source to finish processing. Confirm it appears in the agent's knowledge list and that its status indicates it is ready.
5. If the source fails to process, check that the file is supported or that the URL is accessible, then retry with an approved source. Do not substitute confidential or unapproved documents.

### Step 5: Test Policy Questions
1. Open the **Test your agent** pane. If prompted, start or refresh a test conversation after adding the knowledge source.
2. Ask a question the policy source should answer, for example:  
   `What should I do if my company laptop stops working?`
3. Check that the response is relevant to the policy and does not add unsupported requirements. If citations or source links are shown, open them and verify they point to the expected source.
4. Ask a question that is not covered by the source, for example:  
   `Can I keep my laptop after I leave the company?`
5. Confirm the agent does not guess. It should say that it could not verify the policy and direct the employee to the IT Helpdesk.
6. If either test response is incorrect, confirm the knowledge source is ready, review its contents, and try a question that matches information explicitly stated in the source.

### Step 6: Save and Verify the Lab Result
1. Save or publish draft changes as prompted by Copilot Studio. Publishing is not required for this lab unless your instructor asks you to publish.
2. Return to **IT Asset and Helpdesk Hub** and confirm **IT Helpdesk & Asset Copilot** is listed in the solution.
3. Verify that the agent has the intended instructions and that the IT policy source appears in its **Knowledge** section.

**Time Allocation:** 40 Minutes
