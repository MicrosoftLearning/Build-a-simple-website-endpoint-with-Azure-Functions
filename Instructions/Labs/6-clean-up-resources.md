---
lab:
  title: Clean up resources
  description: Clean up resources
  duration: 5 minutes
  level: 200
  islab: true
---

Complete these steps to avoid ongoing charges from the resources you created in this project.

> [!WARNING]
> Resource deletion is permanent. Verify that you're deleting only resources created for this guided project before you proceed.

## Delete the resource group

Deleting this resource group removes the Function App and its backing storage account.

1.  In the portal search bar, search for **Resource groups** and select **Resource groups**.
2.  Select **rg-gp-functions-endpoint** from the list.
3.  Select **Delete resource group** from the top menu bar.
4.  In the confirmation field, type **rg-gp-functions-endpoint** and select **Delete**.
5.  In the confirmation dialog that appears, select **Delete** again to confirm.
6.  Wait for the notification that confirms the resource group is deleted.

## Delete Application Insights resources

Application Insights and its linked Log Analytics workspace might be in a different resource group than **rg-gp-functions-endpoint**. Use the monitoring resource names you noted earlier to identify exactly what this lab created.

1.  In the portal search bar, search for **Resource groups** and select **Resource groups**.
2.  Locate the resource group that contains the Application Insights resource and/or Log Analytics workspace you noted in the monitoring steps.
3.  Open that resource group and check its contents.
    - If the resource group contains only monitoring resources created for this lab, delete the resource group using the same steps as above.
    - If the resource group contains shared or unrelated resources, delete only the lab monitoring resources (the specific Application Insights resource and linked Log Analytics workspace) and leave all other resources intact.

> [!WARNING]
> Do not delete a Log Analytics workspace used by other services in your subscription—doing so would break monitoring for those services.

## Clean up Cloud Shell files

The function project folder you created in Cloud Shell persists across sessions in a storage account. Remove it to avoid leaving unnecessary files behind.

1.  In the Azure portal, select the **Cloud Shell** icon in the top toolbar.
2.  Run the following command to delete the project folder:

    ```bash
    cd ~ && rm -rf func-gp-endpoint
    ```

## Verify cleanup

1.  In the portal search bar, search for **Resource groups** and confirm **rg-gp-functions-endpoint** no longer appears in the list.
2.  Confirm no additional monitoring resources created for this project remain.
3.  Try browsing to the function endpoint URL you copied earlier. Confirm it no longer responds.
