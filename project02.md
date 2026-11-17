# Project 2: Contact Manager and Welcome Aboard Email

## Objective

Build a simple Power Apps canvas app to create, view, and edit contacts. When a new contact is saved, a Power Automate cloud flow sends a personalized welcome email to that contact.

**Time Allocation:** 60-75 minutes

## What You Will Build

```text
Contact Manager canvas app
    |
    | Save a new contact
    v
CM Contacts table in Dataverse
    |
    | A row is added
    v
Power Automate cloud flow
    |
    v
Welcome email sent through Office 365 Outlook
```

Editing a contact does not trigger a welcome email. The app saves the contact independently of the email flow, so a saved contact does not by itself mean that the email was sent.

## Prerequisites

* Access to `https://make.powerapps.com` and `https://make.powerautomate.com`.
* A training environment with Dataverse, permission to create a solution, table, canvas app, and cloud flow, and the necessary Power Apps and Power Automate licensing.
* Permission to use the Dataverse trigger, including access to the Callback Registration table. Ask your administrator if flow activation fails.
* An Office 365 Outlook mailbox and permission to use its connector.
* A test email address that you own or are authorized to use and can access to check the inbox.

Use the **same environment** throughout. Menu names can vary slightly between Power Platform versions.

> **Email safety:** This project sends real emails. Use only a dedicated test table and an approved test address. Once the flow is on, any new row added to that table in the selected environment can trigger an email, regardless of whether it was created by the app or another client.

## Phase 1: Create the Solution and Table

### Step 1: Create a Solution

1. Open `https://make.powerapps.com` and select your training environment from the environment picker.
2. Select **Solutions** in the left navigation, then **+ New solution**.
3. Enter:
   * **Display name:** `Contact Manager Training`
   * **Publisher:** Select an existing training publisher, or create one if your administrator permits it.
   * **Version:** Leave the default.
4. Select **Create**, then open the new solution.

### Step 2: Create the Contact Table

5. In the solution, select **+ New > Table > Table**. Open the advanced properties if needed.
6. Configure:
   * **Display name:** `CM Contact`
   * **Plural name:** `CM Contacts`
   * **Primary column display name:** `Contact Name`
7. Create or save the table. The publisher prefix in its logical names may differ from other learners' prefixes.
8. Open the table and add these columns. Save each column when prompted.

   | Display name | Data type | Required | Example |
   |---|---|---|---|
   | Contact Name | Primary column, single line of text | Yes | `Alex Example` |
   | Email Address | Single line of text, format **Email** | Business required | Your approved test address |
   | Company | Single line of text | Optional | `Contoso` |
   | Job Title | Single line of text | Optional | `Support Analyst` |

9. Save the table and publish any pending customizations. Wait for the columns to become available.
10. Leave the table empty for now. This ensures you can create your test contact **after** the welcome flow is enabled.

**Checkpoint:** The solution contains a dedicated `CM Contacts` table with the four columns above. Do not use the existing standard Contacts table for this exercise.

## Phase 2: Build the Contact Manager App

### Step 3: Create a Canvas App and Connect the Table

11. Return to the solution and select **+ New > App > Canvas app**.
12. Name the app `Contact Manager`, choose a **Tablet** layout, and select **Create**.
13. In Power Apps Studio, open **Data** in the left pane, select **Add data**, and add the Dataverse table **CM Contacts**.
14. On the first screen, arrange a gallery on the left and an edit form on the right, leaving room for buttons. Use **classic** controls for the buttons, label, and edit form if both modern and classic controls are offered.

For each formula below, select the control, choose the named property from the property dropdown beside the formula bar, and enter the formula. Rename controls using the tree view. If your environment displays a different data-source name, substitute the name shown in the **Data** pane.

### Step 4: Add the Contact List

15. Insert a **blank vertical gallery** and rename it `galContacts`.
16. Set its **Items** property to:

   ```powerfx
   'CM Contacts'
   ```

17. Select the gallery's first item template and insert a **classic Text label** inside it. Set the label's **Text** property to:

   ```powerfx
   ThisItem.'Contact Name'
   ```

18. Set the label's **OnSelect** property to `Select(Parent)`, so clicking the name selects the gallery item.
19. Set `galContacts` **OnSelect** to:

   ```powerfx
   EditForm(frmContact)
   ```

   The formula may show an error until `frmContact` is created in the next step.

### Step 5: Add the Contact Form

20. Insert a **classic Edit form** outside the gallery and rename it `frmContact`.
21. Set the form's **DataSource** to `'CM Contacts'` and its **Item** property to:

   ```powerfx
   galContacts.Selected
   ```

22. Set the form's **DefaultMode** to:

   ```powerfx
   FormMode.New
   ```

23. With the form selected, choose **Edit fields** in the properties pane. Add **Contact Name**, **Email Address**, **Company**, and **Job Title**. Remove unrelated fields if any are shown.
24. Set the form's **OnSuccess** property to:

   ```powerfx
   Notify("Contact saved. Check the flow run history for email delivery.", NotificationType.Success);
   ResetForm(frmContact);
   NewForm(frmContact)
   ```

25. Set the form's **OnFailure** property to:

   ```powerfx
   Notify("Contact could not be saved: " & frmContact.Error, NotificationType.Error)
   ```

### Step 6: Add New and Save Buttons

26. Insert a **classic Button** outside the gallery and form. Set its **Text** to `New contact` and **OnSelect** to:

   ```powerfx
   NewForm(frmContact)
   ```

27. Insert another **classic Button**. Set its **Text** to `Save contact` and **OnSelect** to:

   ```powerfx
   SubmitForm(frmContact)
   ```

28. Save the app. Do not create a contact yet. Close the studio or return to the solution.

**How it works:** `NewForm` prepares a blank record. Selecting an existing name in the gallery switches the form to edit mode. `SubmitForm` validates and saves; `OnSuccess` or `OnFailure` reports the result. Selecting another contact or **New contact** can discard unsaved form edits.

## Phase 3: Create the Welcome Email Flow

### Step 7: Create the Automated Flow

29. In the same solution, select **+ New > Automation > Cloud flow > Automated**.
30. Name the flow `Contact Manager - Welcome Email`.
31. Search for **Microsoft Dataverse - When a row is added, modified or deleted**, select it, and create the flow.
32. Configure the trigger:
   * **Change type:** `Added`
   * **Table name:** `CM Contacts`
   * **Scope:** `Organization`
33. Sign in to the Dataverse connection if prompted. Organization scope watches new rows across the environment; use the dedicated training table only.

### Step 8: Add the Welcome Email

34. Add an action after the trigger: **Office 365 Outlook - Send an email (V2)**.
35. Sign in to the Outlook connection if prompted. This mailbox is the sender.
36. Configure the action:
   * **To:** Select **Email Address** from the Dataverse trigger's dynamic content.
   * **Subject:** `Welcome aboard!`
   * **Body:** Type the following and insert **Contact Name** from the trigger where indicated:

     ```text
     Hello [insert Contact Name dynamic content],

     Welcome aboard! We are delighted to have you with us.

     Your contact details have been added to our contact manager.
     Please reply to this email if you have any questions.

     Best regards,
     Contact Manager Team
     ```

37. Insert the actual dynamic-content token, not the bracketed instruction. Check that the **To** field is also a token, not the literal text `Email Address`.
38. Save the flow. Confirm its details page shows that the flow is **On**; turn it on if necessary.

The trigger starts on new rows only. Existing rows are not sent an email just because the flow is turned on, and edits do not start a new run.

## Phase 4: Test the Complete Project

### Step 9: Create a Contact

39. Open the `Contact Manager` app in Power Apps Studio and select **Preview** (play button).
40. Select **New contact** and enter:
    * **Contact Name:** `Alex Example`
    * **Email Address:** Your approved test email address.
    * **Company:** `Contoso`
    * **Job Title:** `Support Analyst`
41. Select **Save contact**. Confirm that the success notification appears and `Alex Example` appears in the gallery. If necessary, exit preview and reopen it to refresh the list.
42. In the same environment, open the flow's details page in Power Automate and check **Run history**. Open the new run and confirm that the Dataverse trigger and email action succeeded.
43. Check the test inbox, including junk mail, for **Welcome aboard!**. Confirm that the message says `Hello Alex Example`.

### Step 10: Test Editing and Validation

44. Return to the app preview, select `Alex Example` in the gallery, change **Job Title** to `Senior Support Analyst`, and select **Save contact**.
45. Confirm the edited value persists when you select the contact again. Check run history: the edit should not create another welcome-email run.
46. Select **New contact**, leave **Contact Name** or **Email Address** blank, and select **Save contact**. Confirm the form shows a validation error and no new contact or email is created.
47. Save and publish the app when the tests pass. Publishing is needed for other authorized users to run the latest version.

## Expected Outcome

* A contact can be created, viewed, and edited in the app.
* Each new test contact starts a flow run that attempts to send a personalized welcome email.
* Editing an existing contact does not trigger another welcome email.
* Required fields prevent an incomplete contact from being submitted through the app.

## Troubleshooting

| Issue | What to check |
|---|---|
| Table or columns are missing in the app | Check the environment, publish the table customizations, and refresh or re-add the data source. |
| Formula shows an error | Check control names and properties, the table name shown in the Data pane, and whether the edit form was added. Some locales use different formula separators. |
| Contact does not save | Check the form's validation messages, required fields, Dataverse permissions, and the `OnFailure` notification. |
| No flow run appears | Confirm the flow is **On**, the correct environment and table are selected, the trigger uses **Added**, and the row was created after activation. Check trigger permissions. |
| Flow run fails at email | Open the failed action for details; check the recipient and Outlook connection. The saved contact remains in Dataverse. |
| Flow succeeds but email is not in the inbox | Check the recipient, junk folder, and any organizational mail-delivery policies. A successful send action is not proof of inbox delivery. |

Do not repeatedly create the same contact to retry a failed email: each new row can start another send. Inspect the failed run and follow your organization's approved retry process.

## Cleanup and Further Exploration

When finished, turn off the test flow before deleting only the sample contacts and project components you created. Do not remove a shared publisher or other shared resources.

Try adding a search box to the gallery, or add an explicit consent/eligibility check before sending. For production, also plan for duplicate events, retries, delivery monitoring, privacy, and appropriate security roles; this training flow is not an exactly-once delivery system.

## References

* [Understand canvas-app forms](https://learn.microsoft.com/en-us/power-apps/maker/canvas-apps/working-with-forms)
* [Trigger flows when a Dataverse row is added](https://learn.microsoft.com/en-us/power-automate/dataverse/create-update-delete-trigger)
