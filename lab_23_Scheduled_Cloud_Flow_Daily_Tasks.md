# Lab 23: Scheduled Cloud Flow - Daily Automated Tasks

**Objective:** Create a scheduled cloud flow that runs daily to send a summary email of pending support tickets.

## Prerequisites

* Access to Power Automate (`make.powerautomate.com`)
* Access to Dataverse with a **Support Tickets** table (or similar)
* Support Tickets table should have: Title, Status (Open/In Progress/Closed), Priority, Created Date
* Office 365 Outlook configured
* A Power Apps solution to store the flow

## Steps

### Step 1: Create a Scheduled Cloud Flow
1. Open `make.powerautomate.com`
2. Verify correct environment is selected
3. Click **+ Create** → **Cloud flow** → **Scheduled**
4. **Flow name:** `Daily Support Ticket Summary`
5. **Choose how often to run this flow:**
   - **Starting:** Select today's date
   - **Repeat every:** `1` **Day**
6. Click **Create**

### Step 2: Configure the Schedule
7. The flow editor opens with a schedule trigger already configured
8. Click on the schedule step to review:
   - **Starting:** Today's date and time
   - **Repeat every:** `1 Day`
   - **At:** Select time (e.g., 9:00 AM)
9. This ensures the flow runs at the specified time each day
10. Leave **Timezone** as default or adjust to your timezone

### Step 3: Query Open Support Tickets
11. Click **+ New step**
12. Search for `list rows` and select **Microsoft Dataverse — List rows from a table**
13. Configure:
    - **Table name:** Select `Support Tickets`
    - **Filter rows:** Add an OData condition to get only open/pending tickets
      - Click **Filter rows** field
      - Add condition: `statecode eq 1` (adjust based on your status codes)
    - **Sort by:** Select `Created On` (newest first)
    - **Number of rows:** `50`

### Step 4: Count and Categorize Tickets
14. Click **+ New step**
15. Add **Data Operation — Compose** action to structure data
16. **Inputs:** Select `value` from previous step (the records list)
17. Click **+ New step** again
18. Add another **Data Operation — Filter array** action
19. **From:** Select the value from List rows
20. **Filter:** Create conditions to filter by priority (High, Medium, Low)

### Step 5: Build the Summary Email Body
21. Click **+ New step**
22. Search and select **Office 365 Outlook — Send an email (V2)**
23. Configure:
    - **To:** Your email address (or distribution group)
    - **Subject:** `📋 Daily Support Ticket Summary - [Date]`
    - Add date: Use dynamic expression `utcNow('yyyy-MM-dd')`
24. **Body:** Compose a formatted email:
    ```
    <h3>Daily Support Ticket Report</h3>
    
    <p><strong>Report Generated:</strong> [Add expression: utcNow('dddd, MMMM dd, yyyy HH:mm')]</p>
    
    <h4>Summary Statistics</h4>
    <ul>
    <li><strong>Total Open Tickets:</strong> [Count from filter]</li>
    <li><strong>High Priority:</strong> [Number of high priority]</li>
    <li><strong>Medium Priority:</strong> [Number of medium priority]</li>
    <li><strong>Low Priority:</strong> [Number of low priority]</li>
    </ul>
    
    <h4>Pending Tickets (by Priority)</h4>
    <table border="1" cellpadding="5">
    <tr><th>Ticket ID</th><th>Title</th><th>Priority</th><th>Created Date</th></tr>
    <!-- Add table rows with dynamic content -->
    </table>
    
    <p><em>Please review and prioritize accordingly.</em></p>
    ```
25. For dynamic row generation, use an **Apply to each** action to loop through tickets and build the table

### Step 6: Add Apply to Each for Dynamic Table Rows
26. Click **+ New step** (before the email action)
27. Search for `Apply to each` and select **Control — Apply to each**
28. **Select an output from previous steps:** Choose the filtered tickets list
29. Inside the Apply to each loop, add actions to format each row

### Step 7: Save and Test
30. Click **Save** at the top
31. Click **Test** → **Manually** → **Test**
32. The flow runs immediately (don't wait for schedule)
33. Check your email for the summary report

### Step 8: Review Run History
34. After testing, click **Run history**
35. Click on the successful run to see execution details
36. Verify the email was sent with correct data
37. If errors occur, click the failed step to see error details

**Time Allocation:** 25 Minutes

## Expected Outcome

✅ Scheduled flow runs at specified time daily  
✅ Email summary includes all open support tickets  
✅ Ticket counts by priority are accurate  
✅ Email formatted with HTML table for readability

## Common Issues & Solutions

| Issue | Solution |
|-------|----------|
| Flow runs but no email sent | Verify status codes match your Dataverse configuration |
| Email is empty or malformed | Check dynamic content references match actual field names |
| Too many tickets in email | Reduce "Number of rows" or add additional filters |
| Wrong timezone for schedule | Edit flow, adjust "At" time and timezone setting |

## Enhancement Ideas

- Add automatic reminders for SLA breach tickets
- Include attachment with detailed ticket list as CSV
- Send different reports to different teams based on priority
- Add conditional formatting to highlight critical tickets
- Schedule multiple runs per day for high-volume scenarios
