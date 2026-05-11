---
lab:
  title: Exercise - Create the function app
  description: Exercise - Create the function app
  duration: 7 minutes
  level: 200
  islab: true
---

This guided project consists of the following exercises:

 - **Create the Function App**
 - Deploy an HTTP-trigger function
 - Test endpoint and review logs

In this exercise, you create a resource group and deploy a Function App using the serverless Flex Consumption plan. You also confirm monitoring is connected so invocation telemetry is available in later exercises.

This exercise includes the following tasks:

 - Prepare the environment
 - Configure the Function App
 - Verify the deployment

**Outcome:** A running Function App that uses the serverless consumption plan with monitoring configured.

> [!TIP]
> Pause after each major action and confirm the page status before moving on. This habit prevents compounding mistakes.

## Task 1: Prepare the environment

Set up your Azure environment before you begin. You create a resource group to organize all the resources for this project together.

> [!WARNING]
> This project creates Azure resources that may incur charges. Complete the clean-up unit when you're done to avoid unintended expenses.

1.  Sign in to the [Azure portal](https://portal.azure.com) with an account that has permissions to create Function App resources.
2.  In the portal search bar, search for **Resource groups** and select **Resource groups**.
3.  Select **+ Create**. Name the resource group `**rg-gp-functions-endpoint**`, choose your preferred region, and select **Review + create** then **Create**.

## Task 2: Configure the Function App

Set up the Function App with serverless hosting. The Flex Consumption plan ensures you pay only for the execution time of your functions, making it cost-effective for occasional workloads.

1.  In the portal search bar, search for **Function App** and select **Function App**.
2.  Select **Create**.
3.  Select **Flex Consumption** as the hosting option and select **Select**.
4.  On the Basics tab, select **rg-gp-functions-endpoint** as the resource group.
5.  For **Function App name**, enter a globally unique name (for example, `**func-gp-endpoint-**` followed by your initials and a number).
6.  For **Secure unique default host name**, leave the default (**On**).
7.  For **Region**, keep the default or choose your preferred region.
8.  For **Runtime stack**, select **Node.js**.
9.  For **Version**, keep the latest available LTS option.
10. For **Instance size**, leave the default (**2048 MB**).
11. Open the **Monitoring** tab.
12. Confirm **Application Insights** is enabled. Use the default monitoring settings unless your environment requires a specific workspace.
13. Note the Application Insights resource and/or Log Analytics workspace name shown in this step. You use this information during cleanup.
14. Select **Review + create** and then select **Create**.

> [!NOTE]
> **Validation step:** Verify you selected all required settings before deployment: resource group, runtime stack (Node.js), region, Flex Consumption hosting plan, and monitoring enabled.

> [!NOTE]
> Each exercise includes validation steps like this one. Track your results as you go—you'll review them all in the validation unit at the end of this module.

## Task 3: Verify the deployment

Confirm that your Function App deployed successfully and is running.

1.  When deployment completes, select **Go to resource**.
2.  Confirm the Function App Overview page shows a **Running** status.
3.  In the Function App menu, open the monitoring settings and confirm an Application Insights connection is present.

> [!NOTE]
> **Validation step:** Confirm the Function App Overview page shows **Running** status and monitoring is connected.
