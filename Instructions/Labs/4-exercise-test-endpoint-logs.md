---
lab:
  title: Exercise - Test endpoint and review logs
  description: Exercise - Test endpoint and review logs
  duration: 6 minutes
  level: 300
  islab: true
---

This guided project consists of the following exercises:

 - Create the Function App
 - Deploy an HTTP-trigger function
 - **Test endpoint and review logs**

In this exercise, you test your function endpoint, verify monitoring, secure the function with an access key, and review invocation logs. This walks you through the full lifecycle of deploying, securing, and monitoring a serverless function.

This exercise includes the following tasks:

 - Test the HTTP endpoint in a browser
 - Verify the function in the portal
 - Verify monitoring connection
 - Restrict access to the function
 - Test restricted access
 - Review invocation logs

**Outcome:** Endpoint tested successfully and execution logs confirmed in the portal.

## Task 1: Test the HTTP endpoint in a browser

Call your function endpoint to verify it responds correctly. This validates that your serverless function is deployed, running, and reachable from the public internet.

1.  Open a new browser tab.
2.  Paste the **Invoke url** you copied from the deployment output in the previous exercise into the address bar and press **Enter**.
3.  Confirm the browser displays **Hello, world!** (or a similar greeting). This is the default response from the HTTP trigger template.
4.  Open an incognito or private browser window, paste the same URL, and press **Enter**.
5.  Confirm the function responds without requiring any sign-in. This works because you set the authorization level to **Anonymous** when you created the function.

> [!NOTE]
> **Validation step:** Confirm the public endpoint responds correctly with and without query parameters, and works in an incognito window without credentials.

## Task 2: Verify the function in the portal

Confirm the deployed function appears in the Azure portal alongside the Function App you created earlier.

1.  In the Azure portal, in the portal search bar, search for **Function App** and select **Function App**.
2.  Select the Function App you created in the previous exercise.
3.  In the **Overview** page, under **Functions**, confirm **GetStatus** appears in the list with a trigger type of **HTTP**.

> [!NOTE]
> **Validation step:** Confirm the **GetStatus** function appears in the Function App's function list in the portal.

## Task 3: Verify monitoring connection

Before you review logs, confirm that your Function App is connected to Application Insights. In newer Azure portal experiences, monitoring is usually configured during Function App creation and can be verified from the app's monitoring settings.

1.  In the Azure portal, in the portal search bar, search for **Function App** and select **Function App**.
2.  Select the Function App you created in the first exercise.
3.  In the left-hand menu, scroll down to the **Settings** section and select **Application Insights**.
4.  Confirm the Application Insights status shows **Enabled** and a connected resource name is displayed.
5.  Note the connected Application Insights resource name and the linked Log Analytics workspace name (if shown). You use this information during cleanup.
6.  If monitoring is not connected in your environment, select **Turn on Application Insights**, configure the connection, select **Apply**, and select **Yes** when prompted to restart the Function App.

> [!NOTE]
> Azure portal labels and navigation can vary by subscription and experience. Focus on confirming that Application Insights is connected rather than matching exact button text.

> [!NOTE]
> **Validation step:** Confirm Application Insights is connected to the Function App and the linked monitoring resources are noted for cleanup.

## Task 4: Restrict access to the function

Now that monitoring is capturing data, change the authorization level so the function requires a key. This demonstrates how to secure a serverless endpoint.

1.  In the Azure portal, select the **Cloud Shell** icon in the top toolbar to reopen Cloud Shell.
2.  Navigate back to the project folder:

    ```bash
    cd func-gp-endpoint
    ```

3.  Run the following command to change the authorization level from **anonymous** to **function**:

    ```bash
    sed -i "s/authLevel: 'anonymous'/authLevel: 'function'/" src/functions/GetStatus.js
    ```

4.  Verify the change by running:

    ```bash
    grep authLevel src/functions/GetStatus.js
    ```

    Confirm the output shows `authLevel: 'function'`.

5.  Redeploy the function:

    ```bash
    FUNC_APP_NAME=$(az functionapp list --resource-group rg-gp-functions-endpoint --query "[0].name" -o tsv)
    func azure functionapp publish $FUNC_APP_NAME
    ```

6.  Wait for the deployment to complete.

> [!NOTE]
> **Validation step:** Confirm the deployment output shows the **GetStatus** function published successfully.

## Task 5: Test restricted access

Verify that the function now rejects all unauthenticated requests, then use a function key to regain access.

1.  Go back to the browser tab where you previously tested the function URL and refresh the page.
2.  Confirm you receive a **401 Unauthorized** response. The function now requires a key for every request because you changed the authorization level from **anonymous** to **function**.
3.  Return to the Azure portal. In the portal search bar, search for **Function App** and select **Function App**.
4.  Select your Function App, then select **GetStatus** from the function list.
5.  In the top menu, select **Function Keys**.
6.  Copy the **default** key value.
7.  Return to the browser tab with the function URL. Add **?code=** followed by the key you copied to the end of the URL and press **Enter**.
8.  Confirm the function now responds with **Hello, world!** again. The key in the URL proves you are authorized to call the function.

> [!NOTE]
> **Validation step:** Confirm the function returns 401 without a key and succeeds with the key appended.

## Task 6: Review invocation logs

Use Application Insights to review records of your function invocations. The time spent in the previous tasks gave telemetry time to process.

1.  In the portal search bar, search for **Application Insights** and select **Application Insights**.
2.  Select the Application Insights resource connected to your Function App (the name you noted in Task 3).
3.  In the left-hand menu, under the **Investigate** section, select **Transaction search**.
4.  In the **Transaction search** pane, set the time range to **Last 24 hours** (or a range that covers this exercise) and select **Search**.
5.  Review the results and confirm successful invocations with a status of **200**, including calls from Task 1 and Task 5.
6.  Select an invocation entry to view details such as status code, duration, and timestamp.
7.  For deeper analysis, in the left-hand menu, under the **Monitoring** section, select **Logs**.
8.  In the query editor, enter `requests | order by timestamp desc` and select **Run**.
9.  Review the results to see detailed request data for your Function App.

> [!NOTE]
> The 401 Unauthorized responses may not appear as function executions. Azure rejects unauthorized requests at the host level before invoking the function.

> [!NOTE]
> **Validation step:** Confirm Application Insights shows your successful requests and details for at least one invocation.

> [!TIP]
> If invocation logs don't appear immediately, wait up to five minutes and select **Refresh**. Application Insights can take time to process new data.
