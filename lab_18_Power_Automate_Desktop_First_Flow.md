# Lab 18: Your First Power Automate Desktop Flow

**Objective:** Create and run a desktop flow that reads a local support request, writes a formatted copy to a report file, and displays a completion message.

**Time Allocation:** 20-25 minutes

## What You Will Learn

* Create and run an attended desktop flow.
* Use file actions and pass their output between actions with a variable.
* Check the result and troubleshoot a failed run.

## Prerequisites

* A Windows computer with [Power Automate for desktop](https://learn.microsoft.com/en-us/power-automate/desktop-flows/install) installed.
* An account and environment available to sign in and create desktop flows, subject to your organization's policies and licensing.
* Permission to create and modify files in your Documents folder.
* Notepad or another plain-text editor.

This lab runs locally while you are signed in. It does not require Excel, Dataverse, a cloud flow, or a browser extension.

## Scenario

You receive a short support request as a text file. The desktop flow reads the request, adds a heading, and saves it to a report file for review.

```text
request.txt
    |
    v
Read text from file --> %FileContents%
    |
    v
Write text to file --> report.txt
    |
    v
Display message
```

## Steps

### Step 1: Prepare a Test Folder and Input File

1. Open **File Explorer** and go to your **Documents** folder.
2. Create a new folder named `PAD_Lab18`. If that name is already in use, choose a different, empty folder name and use it throughout the lab.
3. Open the new folder. Click the File Explorer address bar and copy its full path. For example, your path might be `C:\Users\YourName\Documents\PAD_Lab18`.
4. Open **Notepad** and enter this sample text:

   ```text
   Request: Laptop cannot connect to Wi-Fi
   Priority: Medium
   Status: Open
   ```

5. In Notepad, select **File > Save as**. Browse to your lab folder.
6. Set **File name** to `request.txt`, **Save as type** to **All files**, and **Encoding** to **UTF-8**. Select **Save**.
7. In File Explorer, enable **View > Show > File name extensions** if needed, and confirm the file is named `request.txt`, not `request.txt.txt`.

Use only sample data in this exercise. Do not select or overwrite an existing report or other work files.

### Step 2: Create a Desktop Flow

8. Open **Power Automate** from the Windows Start menu and sign in if prompted.
9. Select the intended environment in the Power Automate console, if an environment selector is available.
10. Select **New flow**.
11. Enter `Lab 18 - Support Request Report` and select **Create**. The desktop flow designer opens.
12. Find the **Actions** pane on the left and the main flow canvas in the center. Action and field labels may vary slightly by version.

### Step 3: Read the Request File

13. In the **Actions** search box, search for `Read text from file`.
14. Drag **Read text from file** onto the main flow canvas, or double-click it to add it.
15. Configure the action:
    * **File path:** Enter the full path to `request.txt` in your lab folder, for example `C:\Users\YourName\Documents\PAD_Lab18\request.txt`. Replace the example with your actual path.
    * **Store content as:** `Single text value`.
    * **Encoding:** `UTF-8`.
16. Under **Variables produced**, note the output variable name, normally `FileContents`. Keep the default name and select **Save** in the action dialog.

The action stores the whole file in `%FileContents%`. If your designer uses a different output name, use that name in the following steps.

### Step 4: Write a Report

17. Search for `Write text to file` in the **Actions** pane and add it **after** the read action.
18. Configure the action:
    * **File path:** Enter the full path to `report.txt` in the same folder, for example `C:\Users\YourName\Documents\PAD_Lab18\report.txt`.
    * **Text to write:** Enter the following, including the variable:

      ```text
      SUPPORT REQUEST REPORT
      ----------------------
      %FileContents%
      ```

    * **Append new line:** Enabled.
    * **If file exists:** `Append content`.
    * **Encoding:** `UTF-8`.
19. You can type `%FileContents%` or insert the output using the variable picker. Do not type the example path literally.
20. Select **Save** in the action dialog.

The action creates `report.txt` if it does not exist. On subsequent runs it appends another report rather than replacing the previous one.

### Step 5: Display a Completion Message

21. Search for `Display message` and add it **after** the write action.
22. Configure the action:
    * **Message box title:** `Report complete`.
    * **Message to display:** `The request report was saved to your PAD_Lab18 folder.`
    * **Message box icon:** `Information`.
    * **Message box buttons:** `OK`.
    * Leave **Close message box automatically** disabled.
23. Select **Save** in the action dialog.
24. Confirm the actions appear in this order: **Read text from file**, **Write text to file**, **Display message**.

### Step 6: Save and Run

25. Select **Save** in the flow designer.
26. Select **Run** and wait for the completion message. If the message is behind another window, bring it to the front.
27. Select **OK** to let the flow finish.
28. In File Explorer, open `report.txt` in your lab folder. It should contain:

   ```text
   SUPPORT REQUEST REPORT
   ----------------------
   Request: Laptop cannot connect to Wi-Fi
   Priority: Medium
   Status: Open
   ```

29. Confirm that `request.txt` still contains the original sample text.

### Step 7: Test a Second Run and a Failure

30. Edit `request.txt` and change `Priority: Medium` to `Priority: High`. Save the file.
31. Run the flow again and select **OK** on the completion message. Open `report.txt` and verify it contains two report sections: the first with `Medium`, the second with `High`.
32. To test a failure, rename the sample input file from `request.txt` to `request-test.txt` in File Explorer. Run the flow again.
33. Verify that the read action reports a file-not-found error, no completion message appears, and no new report section is appended.
34. Rename `request-test.txt` back to `request.txt` when the test is complete.

## Expected Outcome

* The flow runs on the local Windows computer and displays a completion message after writing the report.
* The report contains the request text and heading; a second run appends a second section.
* A missing input file stops the flow before it writes or displays success.

## Troubleshooting

| Issue | What to check |
|-------|---------------|
| Power Automate does not open or sign-in fails | Confirm the desktop app is installed and contact your administrator about account or environment access. |
| File not found | Check the complete input path and extension; verify `request.txt` is in the selected folder. |
| Cannot write the report | Check the output path and your write permission for the lab folder; close the report in any application that has locked it. |
| Report contains `%FileContents%` rather than the request | Check the read action's produced variable name and insert the variable using the picker. |
| No completion message | Check the designer for an earlier failed action and its error details; also check whether the message is behind another window. |
| Text looks incorrect | Save the input as UTF-8 and select UTF-8 in both file actions. |

## Clean Up and Next Steps

After reviewing the results, you can delete only the sample files and folder you created for this lab, and remove the test flow from the Power Automate console if you no longer need it.

Next, try changing the report heading or input text and run the flow again. Explore a **Display input dialog** to collect a value from the user; handle both **OK** and **Cancel** before writing anything.