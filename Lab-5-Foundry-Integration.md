# Lab 5 - Integrate Copilot Studio with Microsoft Foundry

In this lab, you will learn how to integrate your Copilot Studio agent with Microsoft Foundry while using [custom prompts](https://learn.microsoft.com/en-us/microsoft-copilot-studio/create-custom-prompt), to expand upon the capabilities of your **Real Estate Booking Service** agent.

## Lab Overview

> [!IMPORTANT]
> Microsoft Copilot Studio and Microsoft Foundry are constantly evolving product, with two major releases each year and many incremental updates throughout each calendar year. Whilst every effort has been made to ensure the accuracy of these steps and all screenshots, from time to time, certain instructions or screenshots may no longer resemble the current interface or experience. In case of any issues completing any step, please consult your instructors.

By default, Copilot Studio agents leverage the OpenAI large language models (LLMs), such as GPT 4.1, to process and respond to user queries. However, by integrating with Microsoft Foundry, you can enhance your agent's capabilities by leveraging other models, including:

- DeepSeek
- Grok
- Claude
- Mistral
- Any fine-tuned model, that has been trained on your own data

In this lab, you will configure your **Real Estate Booking Service** agent to use a Microsoft Foundry model within a custom prompt. Once all exercises are complete, you will understand how to set up the integration and modify your agent's behavior using custom prompts.

### 🎯 Goal

- Setup an Azure Resource Group and Microsoft Foundry resource
- Deploy a base model in Microsoft Foundry
- Create a custom prompt in Copilot Studio that uses the Foundry model to generate email content
- Update the agent to use the custom prompt for specific user queries and a tool to send the email
- Test the new custom prompt and tool

### ✅ Prerequisites

- Completion of [Lab 0](./Lab-0-Configure-Environment.md), [Lab 1](./Lab-1-Build-Agent-Copilot-Studio.md), [Lab 2](./Lab-2-Build-Autonomous-Agent.md), [Lab 3](./Lab-3-Enhancing-Agent-With-AI-Capabilities.md) and [Lab 4](./Lab-4-Work-With-MCP-Server.md).
- Access to an Azure subscription with permissions to create resources.

> [!NOTE]
> Your lab instructors may have provided access to a tenant with an Azure subscription for you to use. Please check with them before using your own subscription, as there may be cost implications associated with the resources created during this lab.

### Scenario

Contoso Real Estate is looking to enhance their existing **Real Estate Booking Service** agent by integrating it with Microsoft Foundry. The goal is to leverage a specialized language model to improve the agent's ability to handle complex booking queries and provide more accurate responses to users. As part of this, you will define a custom prompt that will draft an email to be sent to customers after they make a booking.

### ⌛ Length  

This lab will take approximately **30 minutes** to complete.

---

## ✍️ Exercise 1: Setup Azure Resources

### Task 1: Create an Azure Resource Group

[Azure Resource Groups](https://learn.microsoft.com/en-us/azure/azure-resource-manager/management/manage-resource-groups-portal#what-is-a-resource-group) are logical containers that hold related Azure resources, similar to Environments in the Power Platform. In this task, you will create a Resource Group to host your Microsoft Foundry resource.

> [!IMPORTANT]
> If your lab instructors have provided a tenant and subscription for you to use, please check with them to confirm the name of the subscription to use.

1. Open a new browser tab and sign in to the [Azure portal](https://portal.azure.com/) using the same credentials used in the previous lab steps.
2. On the Azure portal home page, click on **Subscriptions**.

    ![Images/Lab-5/Exercise1-Task1-1.png](Images/Lab-5/Exercise1-Task1-1.png)

3. On the **Subscriptions** page, select the subscription you will use for this lab.

    ![Images/Lab-5/Exercise1-Task1-2.png](Images/Lab-5/Exercise1-Task1-2.png)

> [!NOTE]
> In this example, **ESPC 2025_MOD_Administrator** is a subscription provided by the lab instructors. The actual subscription name may differ; consult your instructors in case of any doubts.

4. On the subscription overview page, click on **Resource groups** in the left-hand navigation pane and then select **+ Create**.

    ![Images/Lab-5/Exercise1-Task1-3.png](Images/Lab-5/Exercise1-Task1-3.png)

5. In the **Create a resource group** page, provide the following details and then click **Review + create**:
    - **Subscription**: Verify the correct Azure subscription is selected.
    - **Resource group**: Enter a unique name for the resource group that contains your initials (e.g. `<YourInitials>-Foundry-RG`).
    - **Region**: Leave the default region selected or choose a region close to your location (e.g., `East US`).
    
     ![Images/Lab-5/Exercise1-Task1-4.png](Images/Lab-5/Exercise1-Task1-4.png)

6. On the **Review + create** page, verify the details and click **Create** to create the resource group.

    ![Images/Lab-5/Exercise1-Task1-5.png](Images/Lab-5/Exercise1-Task1-5.png)

7. After a few moments, verify that the resource group was created successfully and then click **Go to resource group**.

    ![Images/Lab-5/Exercise1-Task1-6.png](Images/Lab-5/Exercise1-Task1-6.png)

8. The newly created resource group overview page will be displayed. Leave this page open if you plan to continue to the next task.

**✅ You have successfully created an Azure Resource Group for your Microsoft Foundry resource.**

### Task 2: Create a Microsoft Foundry Resource

1. You should still be on the Resource Group overview page from the previous task. If not, navigate back to the Resource Group you created in Task 1.
2. Click on **+ Create** to add a new resource to the Resource Group.

    ![Images/Lab-5/Exercise1-Task2-1.png](Images/Lab-5/Exercise1-Task2-1.png)

3. On the **Marketplace** page, search for **Microsoft Foundry** in the search bar, then select **Create** -> **Microsoft Foundry** from the search results.

    ![Images/Lab-5/Exercise1-Task2-2.png](Images/Lab-5/Exercise1-Task2-2.png)

4. On the **Create Microsoft Foundry** page, provide the following details and then click **Next**:
    - **Subscription**: Verify the correct Azure subscription is selected.
    - **Resource group**: Ensure the Resource Group you created in Task 1 is selected.
    - **Name**: Enter a unique name for the Foundry resource that contains your initials (e.g. `<YourInitials>-Foundry-Resource`).
    - **Region**: Select either **East US** or **Sweden Central**, depending on your location.
    - **Default project name**: Enter `RealEstateProject`.

    ![Images/Lab-5/Exercise1-Task2-3.png](Images/Lab-5/Exercise1-Task2-3.png)

> [!TIP]
> The latest Foundry capabilities are available within a limited subset of Azure regions. For Europe, generally speaking, **Sweden Central** or **Switzerland North** are recommended. For North America, **East US** or **West US** can be used. Please refer to the [Microsoft Foundry documentation](https://learn.microsoft.com/en-us/azure/ai-foundry/reference/region-support) for the most up-to-date information on region availability.

5. Click **Next** on the **Storage** tab without making any changes.

    ![Images/Lab-5/Exercise1-Task2-4.png](Images/Lab-5/Exercise1-Task2-4.png)

6. Click **Next** on the **Network** tab without making any changes.

    ![Images/Lab-5/Exercise1-Task2-5.png](Images/Lab-5/Exercise1-Task2-5.png)

7. Click **Next** on the **Identity** tab without making any changes.

    ![Images/Lab-5/Exercise1-Task2-6.png](Images/Lab-5/Exercise1-Task2-6.png)

8. Click **Next** on the **Encryption** tab without making any changes.

    ![Images/Lab-5/Exercise1-Task2-7.png](Images/Lab-5/Exercise1-Task2-7.png)

9. Click **Next** on the **Tags** tab without making any changes.

    ![Images/Lab-5/Exercise1-Task2-8.png](Images/Lab-5/Exercise1-Task2-8.png)

10. On the **Review + submit** tab, verify the details and click **Create** to create the Microsoft Foundry resource.

    ![Images/Lab-5/Exercise1-Task2-9.png](Images/Lab-5/Exercise1-Task2-9.png)

11. The resource deployment can take several minutes to complete. Once the deployment is complete, click **Go to resource**.

    ![Images/Lab-5/Exercise1-Task2-10.png](Images/Lab-5/Exercise1-Task2-10.png)

12. On your Microsoft Foundry resource, click on **Go to Foundry portal** to open the Foundry portal in a new browser tab.

    ![Images/Lab-5/Exercise1-Task2-11.png](Images/Lab-5/Exercise1-Task2-11.png)

13. The Foundry portal will open, with your default **RealEstateProject** project selected. Leave this page open if you plan to continue to the next task.

    ![Images/Lab-5/Exercise1-Task2-12.png](Images/Lab-5/Exercise1-Task2-12.png)

**✅ You have successfully created a Microsoft Foundry resource.**

### Task 3: Deploy a Base Model in Microsoft Foundry

1. You should still be on the Foundry portal page from the previous task. If not, navigate back to the Foundry portal and ensure the **RealEstateProject** project is selected.
2. On the **Try the new Microsoft Foundry experience** banner, select **Start building**.

    ![Images/Lab-5/Exercise1-Task3-1.png](Images/Lab-5/Exercise1-Task3-1.png)

3. On the **Choose your compatible project** dialog, select **RealEstateProject** from the dropdown and click **Let's go**.

    ![Images/Lab-5/Exercise1-Task3-2.png](Images/Lab-5/Exercise1-Task3-2.png)

4. On the **Microsoft Foundry (preview)** page, click on **Build** and then **Models** in the left-hand navigation pane.

    ![Images/Lab-5/Exercise1-Task3-3.png](Images/Lab-5/Exercise1-Task3-3.png)

    ![Images/Lab-5/Exercise1-Task3-4.png](Images/Lab-5/Exercise1-Task3-4.png)

5. On the **Models** page, click on **Deploy base model**.

    ![Images/Lab-5/Exercise1-Task3-5.png](Images/Lab-5/Exercise1-Task3-5.png)

6. Explore the list of available models, selecting individual ones to inspect their details and, in particular, the workloads for which they are optimized. For this example, we will be using the **DeepSeek-V3-0324** model. Select this from the list and then click on **Deploy** -> **Default settings**.  Confirm any additional dialog you are presented with.

    ![Images/Lab-5/Exercise1-Task3-6.png](Images/Lab-5/Exercise1-Task3-6.png)

    ![Images/Lab-5/Exercise1-Task3-7.png](Images/Lab-5/Exercise1-Task3-7.png)

> [!NOTE]
> For this exercise, the choice of model is not critical, as we simply wish to understand how to integrate Copilot Studio with Foundry. However, in a real-world scenario, you should select a model that best fits your specific use case and requirements.

> [!IMPORTANT]
> If you are **NOT** using an Azure subscription provided by your instructors, then be sure to review the pricing details for the model you select, to ensure you do not incur any unexpected charges.

7. The model deployment can take several minutes to complete. Once the deployment is complete, you will be taken to the Model details page.

    ![Images/Lab-5/Exercise1-Task3-8.png](Images/Lab-5/Exercise1-Task3-8.png)

8. Test out your new model by first entering the following custom instructions in the **Instructions** field:

    ```
    You are a helpful assistant that drafts professional emails for real estate showings.
    ```

    Then, enter the following prompt in the **Prompt** field:

    ```
    Draft an email to confirm a real estate showing appointment for a client named John Doe on March 15th at 2 PM. Include details about the property address and any necessary documents he should bring.
    ```

    Finally, **Send** the message to see the model's response.

    ![Images/Lab-5/Exercise1-Task3-9.png](Images/Lab-5/Exercise1-Task3-9.png)

> [!TIP]
> If you are using your own subscription and you receive an error similar to the one shown below when sending the message, make sure your user account has been assigned the **Azure AI User** Role Based Access Control (RBAC) role targeting the Foundry resource.

![Images/Lab-5/Exercise1-Task3-10.png](Images/Lab-5/Exercise1-Task3-10.png)

9. After successfully testing your new base model, click on the **Details** tab and copy the following values to a text file, OneNote etc. You will need these values in Exercise 2:
    - **Target URI**
    - **Key**

    ![Images/Lab-5/Exercise1-Task3-11.png](Images/Lab-5/Exercise1-Task3-11.png)

10. Leave the Foundry portal page open if you plan to continue to the next exercise.

**✅ You have successfully deployed a base model in Microsoft Foundry.**

## ✍️ Exercise 2: Create a Custom Prompt in Copilot Studio

### Task 1: Create a Custom Prompt

1. Open a new browser tab and navigate to the [Microsoft Copilot Studio portal](https://copilotstudio.microsoft.com/) using the same credentials used in the previous lab steps. Make sure you are in your **Development** environment.
2. Navigate to the **Real Estate Booking Service** agent you created in Lab 1.
3. Click on the **Tools** tab and then select **+ Add a tool**.

    ![Images/Lab-5/Exercise2-Task1-1.png](Images/Lab-5/Exercise2-Task1-1.png)

4. On the **Add tool** dialog, select **+ New tool** and then choose **Prompt**.

    ![Images/Lab-5/Exercise2-Task1-2.png](Images/Lab-5/Exercise2-Task1-2.png)

    ![Images/Lab-5/Exercise2-Task1-3.png](Images/Lab-5/Exercise2-Task1-3.png)

5. Select the default name of the new prompt and rename it to `Draft Showing Confirmation Email`.

    ![Images/Lab-5/Exercise2-Task1-4.png](Images/Lab-5/Exercise2-Task1-4.png)

    ![Images/Lab-5/Exercise2-Task1-5.png](Images/Lab-5/Exercise2-Task1-5.png)

6. Click on the **GPT-4.1 mini** dropdown and then select the **+** icon next to **Azure AI Foundry models**. You may need to scroll down to see this option.

    ![Images/Lab-5/Exercise2-Task1-6.png](Images/Lab-5/Exercise2-Task1-6.png)

7. On the **Connect a model from Azure AI Foundry** dialog, populate the form with the following values and then press **Connect**:
    - **Model deployment name**: `DeepSeek-V3-0324` (or the name of the model you deployed in Exercise 1, Task 3)
    - **Base model name**: `DeepSeek-V3-0324` (or the name of the model you deployed in Exercise 1, Task 3)
    - **Azure model endpoint URL**: Paste the **Target URI** you copied in Exercise 1, Task 3, step 9.
    - **Key**: Paste the **Key** you copied in Exercise 1, Task 3, step 9.

    ![Images/Lab-5/Exercise2-Task1-7.png](Images/Lab-5/Exercise2-Task1-7.png)


8. The connection may take several moments to establish itself. Confirm that you receive a **Connected** success message and then click **Close** on the dialog. The newly added model should now be selected for the prompt.

    ![Images/Lab-5/Exercise2-Task1-8.png](Images/Lab-5/Exercise2-Task1-8.png)

    ![Images/Lab-5/Exercise2-Task1-9.png](Images/Lab-5/Exercise2-Task1-9.png)

9. In the **Prompt** editor, enter the following prompt. The prompt contains placeholders for inputs, that we will replace in subsequent steps:

    ```
    You are a real estate agent. A client has requested to book a showing for a property. Please confirm the details with the client and send a confirmation email.

    Client Name: {client_name}
    Property Address: {property_address}
    Showing Date and Time: {showing_date_time}

    Draft a professional email confirming the showing appointment and include any necessary documents the client should bring.
    ```

    ![Images/Lab-5/Exercise2-Task1-10.png](Images/Lab-5/Exercise2-Task1-10.png)

10. Highlight the `{client_name}` placeholder in the prompt editor and then press **+ Add content**. Then, select **Text**.

    ![Images/Lab-5/Exercise2-Task1-11.png](Images/Lab-5/Exercise2-Task1-11.png)

    ![Images/Lab-5/Exercise2-Task1-12.png](Images/Lab-5/Exercise2-Task1-12.png)

11. In the dialog that appears, enter the following details and then press **Close**:
    - **Name**: `clientName`
    - **Sample data**: `John Doe`

    ![Images/Lab-5/Exercise2-Task1-13.png](Images/Lab-5/Exercise2-Task1-13.png)

12. Repeat steps 10 and 11 for the `{property_address}` and `{showing_date_time}` placeholders, using the following details:

| Placeholder | Name              | Sample data                  |
|-------------|-------------------|------------------------------|
| `{property_address}` | `propertyAddress` | `123 Main St, Springfield`   |
| `{showing_date_time}` | `showingDateTime` | `March 15th at 2 PM`         |

13. Your finalised prompt should resemble the screenshot below. Click on **Test** to validate the prompt works as expected.

    ![Images/Lab-5/Exercise2-Task1-14.png](Images/Lab-5/Exercise2-Task1-14.png)

    ![Images/Lab-5/Exercise2-Task1-15.png](Images/Lab-5/Exercise2-Task1-15.png)

14. After successfully testing the prompt, click on **Save** to save your changes.

    ![Images/Lab-5/Exercise2-Task1-16.png](Images/Lab-5/Exercise2-Task1-16.png)

15. On the **Add tool** dialog, click **Add and configure** to finish adding the new prompt tool to the agent.

    ![Images/Lab-5/Exercise2-Task1-17.png](Images/Lab-5/Exercise2-Task1-17.png)

16. You will be taking to the configuration page for the new prompt tool. Review the different configuratrion options available, but do **NOT** make any changes.

    ![Images/Lab-5/Exercise2-Task1-18.png](Images/Lab-5/Exercise2-Task1-18.png)

17. Leave Copilot Studio open if you plan to continue to the next task.

**✅ You have successfully created a custom prompt in Copilot Studio that uses a Microsoft Foundry model.**

### Task 2: Add the Office 365 Outlook Tool

> [!IMPORTANT]
> You'll recall in Lab 1, Exercise 5, we created an autonomous agent that leveraged the **Office 365 Outlook** connector to send emails. Can you remember how to add this tool to your agent? As a challenge, try to add the tool without following the steps below. If you get stuck, simply refer to the summarised steps here in this task.

1. You should still be on the configuration page for the `Draft Showing Confirmation Email` prompt tool from the previous task. If not, navigate back to this page.
2. Click on the **Tools** tab and then select **+ Add a tool**.
3. In the search box, type `Send an email` and select the **Send an email (V2) Office 365 Outlook** action.

    ![Images/Lab-5/Exercise2-Task2-1.png](Images/Lab-5/Exercise2-Task2-1.png)

4. On the **send an email V2** dialog, due to our activities in the previous lab, a connection to your email account may have already been established. If so, simply click **Add and configure** to add the tool to the agent. If not, click on **+ Add new connection** and follow the prompts to sign in to your email account and establish the connection. Once complete, click **Add and configure**.

    ![Images/Lab-5/Exercise2-Task2-2.png](Images/Lab-5/Exercise2-Task2-2.png)

    ![Images/Lab-5/Exercise2-Task2-3.png](Images/Lab-5/Exercise2-Task2-3.png)

    ![Images/Lab-5/Exercise2-Task2-4.png](Images/Lab-5/Exercise2-Task2-4.png)

5. The tool will be created after a few moments and you will be redirected to it automatically.
6. Underneath the **Inputs** section, click on the dropdown next to the **To** input and select **Custom Value**.

    ![Images/Lab-5/Exercise2-Task2-5.png](Images/Lab-5/Exercise2-Task2-5.png)

7. In the **Value** input that appears, enter the email address where the message should be sent and then select **Save**. This should be any mailbox which you have access to.

    ![Images/Lab-5/Exercise2-Task2-6.png](Images/Lab-5/Exercise2-Task2-6.png)

8. Leave Copilot Studio open if you plan to continue to the next task.

**✅ You have successfully added the Office 365 Outlook tool to your agent.**

### Task 3: Update the Book a Real Estate Showing Topic

1. You should still be on the configuration page for the **Send an email (V2) Office 365 Outlook** tool from the previous task. If not, navigate back to this page.
2. Click on the **Topics** tab and then select the **Book a real estate showing** topic.

> [!NOTE]
> When testing these lab steps, it was observed that the topic may appear as simply **Book** in the Copilot Studio interface. If so, this is the correct topic to use.

3. Scroll down to the final conversation node and replace the message with the following text:

    ```
    Your booking request has been submitted successfully! We will shortly send you a confirmation email with the details of your showing appointment.
    ```

    ![Images/Lab-5/Exercise2-Task3-1.png](Images/Lab-5/Exercise2-Task3-1.png)

4. Click on the **+ Add action** button below the message you just updated and select **Add a tool** -> **Draft Showing Confirmation Email**.

    ![Images/Lab-5/Exercise2-Task3-2.png](Images/Lab-5/Exercise2-Task3-2.png)

5. Populate the inputs for the `Draft Showing Confirmation Email` tool using the following values:
    - **clientName**: Select the **Name** variable
    - **showingDateTime**: Enter the following Power Fx formula: `Text(Topic.DateTime)`
    - **propertyAddress**: Enter the following Power Fx formula: `First(Topic.ListRecordsWithOrganization.value).contoso_newcolumn`

    ![Images/Lab-5/Exercise2-Task3-3.png](Images/Lab-5/Exercise2-Task3-3.png)

6. Under **Outputs**, click on **Select a variable**. Then, create and set a new variable named `promptOutput` to capture the output of the prompt tool.

    ![Images/Lab-5/Exercise2-Task3-4.png](Images/Lab-5/Exercise2-Task3-4.png)

    ![Images/Lab-5/Exercise2-Task3-5.png](Images/Lab-5/Exercise2-Task3-5.png)

7. After the newly added prompt tool action, click on the **+ Add action** button and select **Add a tool** -> **Send an email (V2)**.

    ![Images/Lab-5/Exercise2-Task3-6.png](Images/Lab-5/Exercise2-Task3-6.png)

8. On the newly added tool, click on **+ Set value** and then select **Subject**.

    ![Images/Lab-5/Exercise2-Task3-7.png](Images/Lab-5/Exercise2-Task3-7.png)

9. In the input that appears, enter `Your Booking Request`.

    ![Images/Lab-5/Exercise2-Task3-8.png](Images/Lab-5/Exercise2-Task3-8.png)

10. Repeat steps 8 and 9 for the **Body** input, but this time, select the `promptOutput.text` variable generated by the previous step.

    ![Images/Lab-5/Exercise2-Task3-9.png](Images/Lab-5/Exercise2-Task3-9.png)

11. Repeat steps 8 and 9 for the **To** input, but this time, select the **EmailAddress** variable.

    ![Images/Lab-5/Exercise2-Task3-10.png](Images/Lab-5/Exercise2-Task3-10.png)

12. **Save** all of your changes to the topic.
13. Leave the topic open if you plan to continue to the next task.

### Task 4: Test the Agent

1. You should still be on the **Book a real estate showing** topic configuration page from the previous task. If not, navigate back to this page.
2. Click on the **Test** tab to open the **Test your agent** pane. Ensure the **Track between topics** option is enabled.

    ![Images/Lab-5/Exercise2-Task4-1.png](Images/Lab-5/Exercise2-Task4-1.png)

3. Initiate a new conversation by typing `I want to book a real estate showing` and pressing **Enter**.
4. Provide the agent with answers to each question, verifying at each stage that the changes in this lab have taken effect:
   - For the date and time, enter a future date and time.
   - For the property type, use one of the synonyms defined earlier (e.g., `Flat` for `Apartment`).
   - For the number of bedrooms, enter `2`; this will ensure the test row created in Lab 1 is retrieved.
   - If prompted, **Allow** the **Office 365 Outlook** connector to access your email account.

    ![Images/Lab-5/Exercise2-Task4-2.png](Images/Lab-5/Exercise2-Task4-2.png)

5. Once the conversation has concluded, check the inbox of the email address you specified in Exercise 2, Task 2, step 7. Verify that you have received an email with the subject `Your Booking Request` and that the body of the email resembles the below.

    ![Images/Lab-5/Exercise2-Task4-3.png](Images/Lab-5/Exercise2-Task4-3.png)

**✅ You have successfully tested the updated agent with the custom prompt and tool.**

#### Additional Challenges

This exercise has focused on the mechanics of working with prompts and tools, and there are several ways you could improve the solution further. Review the following challenges and see if you can implement them in your agent:
- Currently, the prompt is not formatting the email correctly. Is there a way it can be adjusted so that it renders correctly when received in Outlook?
- The email currently generates placeholder text for the sender details. How could you modify the prompt to include your own details?
- Could the prompt be used to also generate a bespoke Subject for the email? If so, how would you implement this in the agent?
- For this scenario, we have used a DeepSeek model. How would the prompt results look if you were to use a different model, such as Grok or Claude?

### Task 5: Clean Up Azure Resources

Before concluding this lab, it is important to clean up the Azure resources you created to avoid any unexpected costs.

> [!IMPORTANT]
> Deleting the resource group will render your agent inoperable. Make sure you are happy to do this before proceeding.

1. Open the [Azure portal](https://portal.azure.com) in a new browser tab.
2. In the search bar, search for and select the Resource Group you created in Exercise 1, Task 1.

    ![Images/Lab-5/Exercise2-Task5-1.png](Images/Lab-5/Exercise2-Task5-1.png)

3. On the Resource Group overview page, click on **Delete resource group**.

    ![Images/Lab-5/Exercise2-Task5-2.png](Images/Lab-5/Exercise2-Task5-2.png)

4. On the **Delete a resource group** pane, type the name of the Resource Group to confirm the deletion and then click **Delete**.

    ![Images/Lab-5/Exercise2-Task5-3.png](Images/Lab-5/Exercise2-Task5-3.png)

5. The deletion process may take several minutes to complete. Once finished, verify that the Resource Group and all associated resources have been deleted successfully.
6. Close the Azure portal tab.

**✅ You have successfully cleaned up the Azure resources created during this lab.**

### Summary

In this lab, you have learned how to: 

- Set up an Azure Resource Group and Microsoft Foundry resource
- Deploy a base model in Microsoft Foundry
- Create a custom prompt in Copilot Studio that uses the Foundry model to generate email content
- Update the agent to use the custom prompt for specific user queries and a tool to send the email
- Test the new custom prompt and tool

**Congratulations, you've finished Lab 5** 🥳