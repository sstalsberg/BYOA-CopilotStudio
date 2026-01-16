# Lab 0 - Configure Lab Environment 

In this lab, you will configure and set up an environment that will be used to complete all subsequent lab exercises. 

## Lab Overview

> [!IMPORTANT]
> Microsoft Copilot Studio is a constantly evolving product, with two major releases each year and many incremental updates throughout each calendar year. Whilst every effort has been made to ensure the accuracy of these steps and all screenshots, from time to time, certain instructions or screenshots may no longer resemble the current interface or experience. In case of any issues completing any step, please consult your instructors.

### 🎯 Goal
- Redeem your [Developer plan license for Power Apps](https://learn.microsoft.com/en-us/power-platform/developer/plan), to create an environment to build your solution - this will be your **Development** environment.
- Create a publisher and solution to store your components, and set this as the [preferred solution](https://learn.microsoft.com/en-us/power-apps/maker/data-platform/preferred-solution) for your environment.

### ✅ Prerequisites

- A Microsoft 365 account
- Permissions to create a [Power Apps Developer environment](https://learn.microsoft.com/en-us/power-platform/developer/create-developer-environment) in your tenant

> [!IMPORTANT]
> Your admin may have disabled the ability to create a Developer environment on your work tenant. If you encounter any error while attempting to create your developer environment, please reach out to them for further assistance or speak to your instructors to arrange for access to an alternative tenant.

### Scenario

You are working as an AI Engineer for Contoso Real Estate, a property management company looking to enhance its customer service experience through digital transformation. Contoso Real Estate specializes in the sale and management of both commercial and residential properties. Currently, customer information is efficiently stored within their Dataverse instance, allowing for streamlined data management. However, the booking process presents a significant challenge.

At present, customers can only request bookings via phone, leading to overwhelmed phone lines and long waiting times. This situation not only frustrates customers, but also risks losing potential business, as many are unable to connect with the office to request services.

To address these issues, Contoso Real Estate is committed to developing a comprehensive digital solution. This solution will empower customers to easily access information about the booking process and submit booking requests online.

You have been tasked with building a solution leveraging Microsoft Copilot Studio and related technologies to address these challenges; but first, you need to set up your development environment.

### ⌛ Length

This lab will take approximately 15 minutes to complete.

---

## ✍️ Exercise 1: Create a Developer Environment

> [!IMPORTANT]
> [Developer environments](https://learn.microsoft.com/en-us/power-platform/developer/create-developer-environment) allow for any Microsoft 365 user (subject to tenant level restrictions) to create up to three environments for personal development, testing and other non-production purposes. Each developer environment comes with up to 2GB of Dataverse storage, and allows you to create apps, automations and more.

### Task 1: Redeem Developer Environment

1. Navigate to the [Power Apps Developer Plan page](https://www.microsoft.com/en-us/power-platform/products/power-apps/free) and click on **Start free**.

    ![Images/Lab-0/Exercise1-Task1-1.png](Images/Lab-0/Exercise1-Task1-1.png)

2. On the **Build apps with the free Power Apps Developer plan** dialog, enter your email address, tick the consent statement and then click on **Start free**.

    ![Images/Lab-0/Exercise1-Task1-2.png](Images/Lab-0/Exercise1-Task1-2.png)

3. When prompted, sign in using your work / Microsoft 365 credentials.
4. On the **Welcome to Power Apps** page, click on **Get started**.

    ![Images/Lab-0/Exercise1-Task1-3.png](Images/Lab-0/Exercise1-Task1-3.png)

4. You will be automatically redirected to the [Power Apps Maker Portal](https://make.powerapps.com). This may take several minutes to complete.

### Task 2: Validate Access

1. Once the Maker portal loads, a new **Development** environment will have been created for you and loaded automatically. You can validate this by checking the following:
   - A banner message reading **This is a developer environment and not meant for production use** is visible on the screen.
   - When selecting the list of Environments, the current environment is shown as **\<Your Name\>'s Environment**.

2. If you see the **Default** environment on the top right of your screen, select your newly created **Development** environment by clicking the environment selector and then select the environment with the title **\<Your Full Name\>’s Environment** (e.g. **John Doe’s Environment**)

    ![Images/Lab-0/Exercise1-Task2-1.png](Images/Lab-0/Exercise1-Task2-1.png)
    ![Images/Lab-0/Exercise1-Task2-2.png](Images/Lab-0/Exercise1-Task2-2.png)

>[!NOTE] 
> The list of environments presented to you will differ, depending on what privileges you have on the tenant.

3. Keep this window open, as we will be returning to it in the next task.

> [!IMPORTANT]
> In all future steps, we will refer to this environment as the **Development** environment. You will use this environment to build your agent.

## ✍️ Exercise 2: Create a Publisher and Solution

We will create a solution to store all our customizations. This will then allow us to more easily transport our changes between different environments and support further application lifecycle management (ALM) activities.

> [!IMPORTANT]
> Always create a new solution to store your customizations, rather than creating an agent or other components separately.

### Task 1: Create Solution & Publisher
1. Navigate to the [Power Apps Maker Portal](https://make.powerapps.com).
2. Ensure your **Development** environment is selected by default; if not, select the current environment name and then click on your **Development** environment.

    ![Images/Lab-0/Exercise2-Task1-1.png](Images/Lab-0/Exercise2-Task1-1.png)

3. Click on **Solutions**:

    ![Images/Lab-0/Exercise2-Task1-2.png](Images/Lab-0/Exercise2-Task1-2.png)

4. On the **Solutions** page, select **New solution**:

    ![Images/Lab-0/Exercise2-Task1-3.png](Images/Lab-0/Exercise2-Task1-3.png)

5. On the **Create a solution** pane, enter the following details and then click on the **Create** button:
    - **Display name**: `Real Estate Agent`
    - **Name**: `RealEstateAgent`
    - **Publisher**: Select **New publisher**, enter the following details and then press **Save**. Then, select the newly created publisher in the dropdown:
        - **Display name**: `Contoso Real Estate`
        - **Name**: `ContosoRealEstate`
        - **Description**: `Publisher for Contoso Real Estate`
        - **Prefix**: `contoso`
        - **Choice value prefix**: `76908`
    - **Version**: `1.0.0.0`

    ![Images/Lab-0/Exercise2-Task1-4.png](Images/Lab-0/Exercise2-Task1-4.png)

    ![Images/Lab-0/Exercise2-Task1-5.png](Images/Lab-0/Exercise2-Task1-5.png)

6. You will be taken through automatically to the new Solution. Leave this page open, as we will continue from here in the next task.

    ![Images/Lab-0/Exercise2-Task1-6.png](Images/Lab-0/Exercise2-Task1-6.png)

### Task 2: Set Preferred Solution

To ensure any additional customizations we create outside of a solution are redirected to the correct location, we will proceed to set our new **Real Estate Agent** solution as the environment default. It’s recommended to always configure this on a per environment basis.

1. You should still be on the **Real Estate Agent** solution page from the previous task. Click on the **Back** arrow to return to the **Solutions** page.

    ![Images/Lab-0/Exercise2-Task2-1.png](Images/Lab-0/Exercise2-Task2-1.png)

2.	Under **Solutions** in the Maker portal, select **Manage** under **Current preferred solution**.

    ![Images/Lab-0/Exercise2-Task2-2.png](Images/Lab-0/Exercise2-Task2-2.png)

3. Select **Real Estate Agent (contoso)** under **Unless otherwise specified, save my changes in** and click **Apply**.

    ![Images/Lab-0/Exercise2-Task2-3.png](Images/Lab-0/Exercise2-Task2-3.png)

4. Verify that the **Current preferred solution** now shows **Real Estate Agent**.

    ![Images/Lab-0/Exercise2-Task2-4.png](Images/Lab-0/Exercise2-Task2-4.png)

## ✍️ Exercise 3: Import Contact Data

For [Lab 1](./Lab-1-Build-Agent-Copilot-Studio.md), we will be working with **Booking Request** and **Real Estate Booking** tables in Dataverse; for that, we would need to have some **Contact** sample data installed in our environment. In this exercise, we will import this data into our Dataverse instance using [dataflows](https://learn.microsoft.com/en-us/power-apps/maker/data-platform/create-and-use-dataflows).

1. Download the [Contacts.csv](./Assets/Lab-0/Contacts.csv) file to your local machine.
2. Open a new browser tab and navigate to the [Power Apps Maker Portal](https://make.powerapps.com).
3. In the **Power Apps Maker Portal**, click on **Tables** from the left-hand navigation menu and then click on **Import** -> **Import data with Dataflows**:

    ![Images/Lab-0/Exercise3-1](Images/Lab-0/Exercise3-1.png)

4. In the **Choose data source** page, drag and drop or browse and select the [Contacts.csv](./Assets/Lab-0/Contacts.csv) file you downloaded in step 1. Once selected, click on **Next**:

    ![Images/Lab-0/Exercise3-2](Images/Lab-0/Exercise3-2.png)

5. On the **Connect to data source** page, click on **Sign in**. If prompted, sign in with your work or school account. Once you've signed in, click on **Next**:

    ![Images/Lab-0/Exercise3-3](Images/Lab-0/Exercise3-3.png)

    ![Images/Lab-0/Exercise3-4](Images/Lab-0/Exercise3-4.png)

> [!IMPORTANT]
> If you receive a warning message relating to licensing or have not used OneDrive for Business previously, click on **Back**, open a new browser tab and navigate to [OneDrive for Business](https://m365.cloud.microsoft/onedrive/). Sign in with your work or school account if prompted. Once done, return to the previous tab with the Maker portal and repeat steps 4-5.

6. On the **Preview file data** screen, review the data that will be imported and then click on **Next**:

    ![Images/Lab-0/Exercise3-5](Images/Lab-0/Exercise3-5.png)

7. On the Power Query editor page, click on the icon next to **birthdate** and change the data type to **Date**. If prompted with a **Change column type** dialog, click on **Add new step**. Once done, click on **Next**:

    ![Images/Lab-0/Exercise3-6](Images/Lab-0/Exercise3-6.png)

    ![Images/Lab-0/Exercise3-7](Images/Lab-0/Exercise3-7.png)

    ![Images/Lab-0/Exercise3-8](Images/Lab-0/Exercise3-8.png)

8. On the **Map tables** page, populate the details as follows and then select **Next**:
    - **Load settings**: Select **Load to existing table**
    - **Destination table**: Select **Contact**

    ![Images/Lab-0/Exercise3-9](Images/Lab-0/Exercise3-9.png)

    - Expand the **Column mapping** heading and then select **Mapped**. Review the columns. There should be four columns that will be updated; if so, click **Next**. However, if these are not mapped automatically, please follow the instructions below.

    ![Images/Lab-0/Exercise3-10](Images/Lab-0/Exercise3-10.png)
    
    - Map the **Source column**'s and **Destination column**'s as indicated below:
        - birthdate -> BirthDate
        - emailaddress1 -> EMailAddress1
        - firstname -> FirstName
        - lastname -> LastName

    ![Images/Lab-0/Exercise3-11](Images/Lab-0/Exercise3-11.png)

    ![Images/Lab-0/Exercise3-12](Images/Lab-0/Exercise3-12.png)

    ![Images/Lab-0/Exercise3-13](Images/Lab-0/Exercise3-13.png)

9. On the **Refresh settings** page, ensure the **Refresh manually** option is selected and then click on **Publish**:

    ![Images/Lab-0/Exercise3-14](Images/Lab-0/Exercise3-14.png)

10. You will be taken back to the Maker portal. Click on **More** and then select **Dataflows**:

    ![Images/Lab-0/Exercise3-15](Images/Lab-0/Exercise3-15.png)

11. On the **Dataflows** page, you will see a single dataflow, which will display **In progress** under the **Next refresh** heading. Wait a few minutes, refresh the page and confirm that the refresh has completed successfully:

    ![Images/Lab-0/Exercise3-16](Images/Lab-0/Exercise3-16.png)

    ![Images/Lab-0/Exercise3-17](Images/Lab-0/Exercise3-17.png)

12. The **Contact** data is now imported successfully.

**Congratulations, you've finished Lab 0** 🥳