# Setting Up the Test Results Importer Extension for Azure DevOps by Solidify

This document will guide first-time users through the process of setting up and using the **Test Results Importer** extension for Azure DevOps by Solidify. This extension allows users to import test scenarios into Azure DevOps test plans, making it easier to manage test cases.

## Prerequisites

- An Azure DevOps Organization.
- Basic understanding of Azure DevOps Pipelines.

## Step-by-Step Guide

### Step 1: Install the Extension

1. Navigate to the product page at the **Azure DevOps Marketplace**: <https://marketplace.visualstudio.com/items?itemName=solidify.import-test-results>.
2. Select **Get**.
3. Choose the Azure DevOps organization where you want to install the extension.
4. Follow the prompts to complete the installation.

### Step 2: Obtain and Activate the License

1. Contact us at <support.tri@solidify.dev> to claim your free trial. You will receive a license file which you will need in order to proceed with the next step.
2. Once you receive the license file, navigate to the **Azure DevOps Organization Settings**.
3. Go to the **Extensions** section and find **Test Results Importer**.
4. Click on **Manage**.
5. Under the **Enter License Manually** section, there is a text field. Paste the contents of the json file in its entirety here.

![image](https://github.com/user-attachments/assets/66fbbf5a-6b15-4cb5-a5fa-4e4f22b4ddb8)

6. Click **Activate license**. Verify that the extension has been activated. There should be a green text saying *License activated*. If not, contact us at <support.tri@solidify.dev>.

### Step 3: Create a New Test Plan

1. Go to the **Azure DevOps** project where you want to use the extension.
2. Navigate to **Test Plans**.

![image](https://github.com/user-attachments/assets/a5ef9b8c-7bf8-481c-98a3-eb4643aa6679)

3. Click on **New Test Plan**.

![image](https://github.com/user-attachments/assets/fcb04145-1817-48f0-897f-a4f6cb0d62fd)

4. Provide a name for the Test Plan (e.g., **Sample Test Plan**) and select the area path and iteration path as needed.

![image](https://github.com/user-attachments/assets/cdad38c4-8999-48fe-af87-d25b2421661d)

5. Click **Create**.
6. **Note down the name** of the Test Plan as it will be needed when configuring the pipeline task.

### Step 4: Create a new Git Repository

1. Go to **Repos** and create a **new repository**.

![image](https://github.com/user-attachments/assets/d528529e-ae13-4914-a1f9-b4ecc7bc911a)

2. Provide a **Repository name** and click **Create**

![image](https://github.com/user-attachments/assets/f7c8c623-b85f-497c-9594-949c2459be7d)

Once the repository is created, let us add our test results file (for the purpose of the demo, we will keep a **JUnit** test results file in our git repository. Keep in mind that in a production scenario, you would have the Test Results Importer as a separate step in the build automation after your tests).

3. In your git repository, click **New** > **File**

![image](https://github.com/user-attachments/assets/cd34def8-680c-4250-9501-bf683b4e630e)

4. Enter the file name `test-results.xml` and click create.

![image](https://github.com/user-attachments/assets/3b9a959c-3535-450b-af50-d1da15af79aa)

5. Below is a simple example of a JUnit test results XML file for demo purposes. Copy and paste the contents into your `test-results.xml` file and click **Commit**:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<testsuites>
  <testsuite name="UserManagementSuite">
    <testcase name="CreateUserTest" classname="UserManagementSuite" time="0.01"/>
    <testcase name="UpdateUserTest" classname="UserManagementSuite" time="0.02"/>
    <testcase name="DeleteUserTest" classname="UserManagementSuite" time="0.03">
      <failure type="AssertionError">Expected value 'true' but got 'false'</failure>
    </testcase>
    <testcase name="ViewUserDetailsTest" classname="UserManagementSuite" time="0.02"/>
    <testcase name="SearchUserTest" classname="UserManagementSuite" time="0.02"/>
    <testcase name="UserPermissionsTest" classname="UserManagementSuite" time="0.02">
      <failure type="AssertionError">Expected value 'true' but got 'false'</failure>
    </testcase>
    <testcase name="UserAccessTest" classname="UserManagementSuite" time="0.02">
      <failure type="NullPointerException">NullPointerException occurred</failure>
    </testcase>
  </testsuite>
  <testsuite name="ProductCatalogSuite">
    <testcase name="AddProductTest" classname="ProductCatalogSuite" time="0.01"/>
    <testcase name="UpdateProductTest" classname="ProductCatalogSuite" time="0.02">
      <error type="NullPointerException">NullPointerException occurred</error>
    </testcase>
    <testcase name="DeleteProductTest" classname="ProductCatalogSuite" time="0.03"/>
    <testcase name="ViewProductDetailsTest" classname="ProductCatalogSuite" time="0.02"/>
    <testcase name="SearchProductTest" classname="ProductCatalogSuite" time="0.02"/>
    <testcase name="ProductAvailabilityTest" classname="ProductCatalogSuite" time="0.02">
      <failure type="AssertionError">Expected value 'true' but got 'false'</failure>
    </testcase>
    <testcase name="ProductPricingTest" classname="ProductCatalogSuite" time="0.02">
      <failure type="NullPointerException">NullPointerException occurred</failure>
    </testcase>
  </testsuite>
  <testsuite name="OrderProcessingSuite">
    <testcase name="PlaceOrderTest" classname="OrderProcessingSuite" time="0.01"/>
    <testcase name="CancelOrderTest" classname="OrderProcessingSuite" time="0.02"/>
    <testcase name="ProcessOrderTest" classname="OrderProcessingSuite" time="0.03"/>
    <testcase name="ViewOrderDetailsTest" classname="OrderProcessingSuite" time="0.02"/>
    <testcase name="SearchOrderTest" classname="OrderProcessingSuite" time="0.02"/>
    <testcase name="OrderValidationTest" classname="OrderProcessingSuite" time="0.02">
      <failure type="AssertionError">Expected value 'true' but got 'false'</failure>
    </testcase>
    <testcase name="OrderStatusTest" classname="OrderProcessingSuite" time="0.02">
      <failure type="NullPointerException">NullPointerException occurred</failure>
    </testcase>
  </testsuite>
  <testsuite name="InventoryManagementSuite">
    <testcase name="AddStockTest" classname="InventoryManagementSuite" time="0.01"/>
    <testcase name="UpdateStockTest" classname="InventoryManagementSuite" time="0.02"/>
    <testcase name="DeleteStockTest" classname="InventoryManagementSuite" time="0.03">
      <failure type="AssertionError">Expected value 'true' but got 'false'</failure>
    </testcase>
    <testcase name="ViewStockDetailsTest" classname="InventoryManagementSuite" time="0.02"/>
    <testcase name="SearchStockTest" classname="InventoryManagementSuite" time="0.02"/>
    <testcase name="StockAvailabilityTest" classname="InventoryManagementSuite" time="0.02">
      <failure type="AssertionError">Expected value 'true' but got 'false'</failure>
    </testcase>
    <testcase name="StockUpdateTest" classname="InventoryManagementSuite" time="0.02">
      <failure type="NullPointerException">NullPointerException occurred</failure>
    </testcase>
  </testsuite>
  <testsuite name="PaymentProcessingSuite">
    <testcase name="ProcessPaymentTest" classname="PaymentProcessingSuite" time="0.01"/>
    <testcase name="RefundPaymentTest" classname="PaymentProcessingSuite" time="0.02"/>
    <testcase name="ViewPaymentDetailsTest" classname="PaymentProcessingSuite" time="0.02"/>
    <testcase name="SearchPaymentTest" classname="PaymentProcessingSuite" time="0.02"/>
    <testcase name="PaymentValidationTest" classname="PaymentProcessingSuite" time="0.02">
      <failure type="AssertionError">Expected value 'true' but got 'false'</failure>
    </testcase>
    <testcase name="PaymentStatusTest" classname="PaymentProcessingSuite" time="0.02">
      <failure type="NullPointerException">NullPointerException occurred</failure>
    </testcase>
  </testsuite>
</testsuites>
```

6. Provide a meaningful commit message (e.g., **Add Test Results file**), and click **Commit**.

### Step 5: Create a New YAML Pipeline in the git repository

1. Click on **Pipelines** in the left menu and select **New Pipeline**.
2. Under the **Where is your code?** prompt, choose **Azure DevOps Git (YAML)** and select your new repository.
3. Under the **Configure** section, select **Starter pipeline**.

### Step 6: Add the "Import Test Scenarios" Task Using the Assistant

1. In the YAML pipeline editor, click on **Show assistant** on the right side.

![image](https://github.com/user-attachments/assets/7de534b3-8c49-4576-be61-0c753889455e)

2. Search for **Import Test Scenarios**, and select the **Import Test Scenarios** task from the list.

![image](https://github.com/user-attachments/assets/0fc4251a-054c-4c96-a9dc-6afcd037d593)

3. Fill in the required parameters:
   - **Testresults filepath**: The root path to your testresults file, default: `$(System.DefaultWorkingDirectory)`
   - **Specific Filename**: The name of your test results file (without the file extension, e.g. .xml or .json)
   - **Name of TestPlan**: Enter the **name of the Test Plan** created in Step 3 (e.g., **Sample Test Plan**).
   - **File type** (`JUnit`/`Gherkin`/`TestNG`)

  This is all the configuration required! However, you can see that there are a lot more options to try out (you can hover over the info icon for each parameter to see an informative tooltip):

![image](https://github.com/user-attachments/assets/d7127ffc-3f36-4975-b2b6-443a5cdaa75e)

### Step 7: Save the Pipeline

1. Click **Save** in the pipeline editor.
2. Provide a meaningful commit message (e.g., **Add Import Test Scenarios task**).
3. Click **Save and run**.

### Step 8: Run the Pipeline

1. After saving, the pipeline will automatically start running.
2. If it does not start automatically, click **Run pipeline** manually.

![image](https://github.com/user-attachments/assets/39ec3661-a9fe-42bd-a0ce-b57268d8718a)

3. Wait for the pipeline to complete. You might need to refresh the web page to see the status of the pipeline.

### Step 9: Review the Pipeline Log

1. Click on the pipeline run and navigate to the **Logs** section.

![image](https://github.com/user-attachments/assets/9931f5ea-1ef1-4b1b-bb2d-1af7f05391ed)

2. Review the output for the **Import Test Scenarios** task.
3. Ensure that the task logs indicate a successful import of the test scenarios.

![image](https://github.com/user-attachments/assets/43f5cee8-7a81-4fa1-8008-127b50cf0ee5)

### Step 10: Verify Test Plan and Test Cases

1. Go back to the **Test Plans** section of your Azure DevOps project.
2. Open the **Sample Test Plan** (or the name you specified).
3. Verify that the test cases from the JUnit test data file have been imported.
4. Ensure that the structure (e.g., test cases, test steps) is correctly represented as expected.

![image](https://github.com/user-attachments/assets/4bb3492c-4484-4d03-bee4-9c018e4d7859)

### Summary

This guide covers the process of setting up and using the **Test Results Importer** extension for the first time, from installation and activation to running a pipeline and verifying imported test cases. Use the example JUnit test data file to quickly verify the functionality and ensure that the test cases are imported correctly into Azure DevOps. Happy testing!
