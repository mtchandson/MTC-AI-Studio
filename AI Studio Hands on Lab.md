# Prompt flow in Azure AI Studio

## Prerequisites

- An Azure subscription - [Create one for free](https://azure.microsoft.com/free/cognitive-services).

- Access granted to Azure OpenAI in the desired Azure subscription.

  Currently, access to this service is granted only by application. You can apply for access to Azure OpenAI by completing the form at https://aka.ms/oai/access. Open an issue on this repo to contact us if you have an issue.

- You need an Azure AI resource and your user role must be **Azure AI Developer**, **Contributor**, or **Owner** on the Azure AI resource. For more information, see [Azure AI resources](https://learn.microsoft.com/en-us/azure/ai-studio/concepts/ai-resources) and [Azure AI roles](https://learn.microsoft.com/en-us/azure/ai-studio/concepts/rbac-ai-studio).

  - If your role is **Contributor** or **Owner**, you can [create an Azure AI resource in this tutorial](https://learn.microsoft.com/en-us/azure/ai-studio/tutorials/deploy-copilot-ai-studio#create-an-azure-ai-project-in-azure-ai-studio).
  - If your role is **Azure AI Developer**, the Azure AI resource must already be created.

- Your subscription needs to be below your [quota limit](https://learn.microsoft.com/en-us/azure/ai-studio/how-to/quota) to [deploy a new model in this tutorial](https://learn.microsoft.com/en-us/azure/ai-studio/tutorials/deploy-copilot-ai-studio#deploy-a-chat-model). Otherwise you already need to have a [deployed chat model](https://learn.microsoft.com/en-us/azure/ai-studio/how-to/deploy-models-openai).

- You need a local copy of product and customer data. The [Azure/aistudio-copilot-sample repository on GitHub](https://github.com/Azure/aistudio-copilot-sample/tree/main/data) contains sample retail customer and product information that's relevant for this tutorial scenario. Clone the repository or copy the files from [1-customer-info](https://github.com/Azure/aistudio-copilot-sample/tree/main/data/1-customer-info) and [3-product-info](https://github.com/Azure/aistudio-copilot-sample/tree/main/data/3-product-info).



## Create an Azure AI project in Azure AI Studio

Your Azure AI project is used to organize your work and save state while building your copilot. During this tutorial, your project contains your data, prompt flow runtime, evaluations, and other resources. For more information about the Azure AI projects and resources model, see [Azure AI resources](https://learn.microsoft.com/en-us/azure/ai-studio/concepts/ai-resources).

To create an Azure AI project in Azure AI Studio, follow these steps:

1. Sign in to [Azure AI Studio](https://ai.azure.com/) and go to the **Build** page from the top menu.

2. Select **+ New project**.

3. Enter a name for the project.

4. Select an Azure AI resource from the dropdown to host your project. If you don't have access to an Azure AI resource yet, select **Create a new resource**.

   [![Screenshot of the project details page within the create project dialog.](https://learn.microsoft.com/en-us/azure/ai-studio/media/tutorials/copilot-deploy-flow/create-project-details.png)](https://learn.microsoft.com/en-us/azure/ai-studio/media/tutorials/copilot-deploy-flow/create-project-details.png#lightbox)

    Note

   To create an Azure AI resource, you must have **Owner** or **Contributor** permissions on the selected resource group. It's recommended to share an Azure AI resource with your team. This lets you share configurations like data connections with all projects, and centrally manage security settings and spend.

5. If you're creating a new Azure AI resource, enter a name.

   [![Screenshot of the create resource page within the create project dialog.](https://learn.microsoft.com/en-us/azure/ai-studio/media/tutorials/copilot-deploy-flow/create-project-resource.png)](https://learn.microsoft.com/en-us/azure/ai-studio/media/tutorials/copilot-deploy-flow/create-project-resource.png#lightbox)

6. Select your **Azure subscription** from the dropdown. Choose a specific Azure subscription for your project for billing, access, or administrative reasons. For example, this grants users and service principals with subscription-level access to your project.

7. Leave the **Resource group** as the default to create a new resource group. Alternatively, you can select an existing resource group from the dropdown.

    Tip

   Especially for getting started it's recommended to create a new resource group for your project. This allows you to easily manage the project and all of its resources together. When you create a project, several resources are created in the resource group, including an Azure AI resource, a container registry, and a storage account.

8. Enter the **Location** for the Azure AI resource and then select **Next**. The location is the region where the Azure AI resource is hosted. The location of the Azure AI resource is also the location of the project.

    Note

   Azure AI resources and services availability differ per region. For example, certain models might not be available in certain regions. The resources in this tutorial are created in the **East US 2** region.

9. Review the project details and then select **Create a project**.

Once a project is created, you can access the **Tools**, **Components**, and **Settings** assets in the left navigation panel.



## Deploy a chat model

Follow these steps to deploy an Azure OpenAI chat model for your copilot.

1. Sign in to [Azure AI Studio](https://ai.azure.com/) with credentials that have access to your Azure OpenAI resource. During or after the sign-in workflow, select the appropriate directory, Azure subscription, and Azure OpenAI resource. You should be on the Azure AI Studio **Home** page.

2. Select **Build** from the top menu and then select **Deployments** > **Create**.

   [![Screenshot of the deployments page with a button to create a new project.](https://learn.microsoft.com/en-us/azure/ai-studio/media/tutorials/copilot-deploy-flow/deploy-create.png)](https://learn.microsoft.com/en-us/azure/ai-studio/media/tutorials/copilot-deploy-flow/deploy-create.png#lightbox)

3. On the **Select a model** page, select the model you want to deploy from the **Model** dropdown. For example, select **gpt-35-turbo-16k**. Then select **Confirm**.

   [![Screenshot of the model selection page.](https://learn.microsoft.com/en-us/azure/ai-studio/media/tutorials/copilot-deploy-flow/deploy-gpt-35-turbo-16k.png)](https://learn.microsoft.com/en-us/azure/ai-studio/media/tutorials/copilot-deploy-flow/deploy-gpt-35-turbo-16k.png#lightbox)

4. On the **Deploy model** page, enter a name for your deployment, and then select **Deploy**. After the deployment is created, you see the deployment details page. Details include the date you created the deployment and the created date and version of the model you deployed.

5. On the deployment details page from the previous step, select **Open in playground**.

   [![Screenshot of the GPT chat deployment details.](https://learn.microsoft.com/en-us/azure/ai-studio/media/tutorials/copilot-deploy-flow/deploy-gpt-35-turbo-16k-details.png)](https://learn.microsoft.com/en-us/azure/ai-studio/media/tutorials/copilot-deploy-flow/deploy-gpt-35-turbo-16k-details.png#lightbox)

For more information about deploying models, see [how to deploy models](https://learn.microsoft.com/en-us/azure/ai-studio/how-to/deploy-models-openai).



## Chat in the playground without your data

In the [Azure AI Studio](https://ai.azure.com/) playground you can observe how your model responds with and without your data. In this section, you test your model without your data. In the next section, you add your data to the model to help it better answer questions about your products.

1. In the playground, make sure that **Chat** is selected from the **Mode** dropdown. Select your deployed GPT chat model from the **Deployment** dropdown.

   [![Screenshot of the chat playground with the chat mode and model selected.](https://learn.microsoft.com/en-us/azure/ai-studio/media/tutorials/copilot-deploy-flow/playground-chat.png)](https://learn.microsoft.com/en-us/azure/ai-studio/media/tutorials/copilot-deploy-flow/playground-chat.png#lightbox)

2. In the **System message** text box on the **Assistant setup** pane, provide this prompt to guide the assistant: "You're an AI assistant that helps people find information." You can tailor the prompt for your scenario. For more information, see [prompt samples](https://learn.microsoft.com/en-us/azure/ai-studio/how-to/models-foundation-azure-ai#prompt-samples).

3. Select **Apply changes** to save your changes, and when prompted to see if you want to update the system message, select **Continue**.

4. In the chat session pane, enter the following question: "How much do the TrailWalker hiking shoes cost", and then select the right arrow icon to send.

   [![Screenshot of the first chat question without grounding data.](https://learn.microsoft.com/en-us/azure/ai-studio/media/tutorials/copilot-deploy-flow/chat-without-data.png)](https://learn.microsoft.com/en-us/azure/ai-studio/media/tutorials/copilot-deploy-flow/chat-without-data.png#lightbox)

5. The assistant replies that it doesn't know the answer. The model doesn't have access to product information about the TrailWalker hiking shoes.

   [![Screenshot of the assistant's reply without grounding data.](https://learn.microsoft.com/en-us/azure/ai-studio/media/tutorials/copilot-deploy-flow/assistant-reply-not-grounded.png)](https://learn.microsoft.com/en-us/azure/ai-studio/media/tutorials/copilot-deploy-flow/assistant-reply-not-grounded.png#lightbox)

In the next section, you'll add your data to the model to help it answer questions about your products.



## Add your data and try the chat model again

You need a local copy of example product information. For more information and links to example data, see the [prerequisites](https://learn.microsoft.com/en-us/azure/ai-studio/tutorials/deploy-copilot-ai-studio#prerequisites).

You upload your local data files to Azure Blob storage and create an Azure AI Search index. Your data source is used to help ground the model with specific data. Grounding means that the model uses your data to help it understand the context of your question. You're not changing the deployed model itself. Your data is stored separately and securely in your Azure subscription. For more information, see [Azure OpenAI on your data](https://learn.microsoft.com/en-us/azure/ai-services/openai/concepts/use-your-data).

Follow these steps to add your data to the playground to help the assistant answer questions about your products.

1. If you aren't already in the [Azure AI Studio](https://ai.azure.com/) playground, select **Build** from the top menu and then select **Playground** from the collapsible left menu.

2. On the **Assistant setup** pane, select **Add your data (preview)** > **+ Add a data source**.

   [![Screenshot of the chat playground with the option to add a data source visible.](https://learn.microsoft.com/en-us/azure/ai-studio/media/tutorials/copilot-deploy-flow/add-your-data.png)](https://learn.microsoft.com/en-us/azure/ai-studio/media/tutorials/copilot-deploy-flow/add-your-data.png#lightbox)

3. In the **Data source** page that appears, select **Upload files** from the **Select data source** dropdown.

   [![Screenshot of the product data source selection options.](https://learn.microsoft.com/en-us/azure/ai-studio/media/tutorials/copilot-deploy-flow/add-your-data-source.png)](https://learn.microsoft.com/en-us/azure/ai-studio/media/tutorials/copilot-deploy-flow/add-your-data-source.png#lightbox)

    Tip

   For data source options and supported file types and formats, see [Azure OpenAI on your data](https://learn.microsoft.com/en-us/azure/ai-services/openai/concepts/use-your-data).

4. Enter *product-info* as the name of your product information index.

   [![Screenshot of the resources and information required to upload files.](https://learn.microsoft.com/en-us/azure/ai-studio/media/tutorials/copilot-deploy-flow/add-your-data-source-details.png)](https://learn.microsoft.com/en-us/azure/ai-studio/media/tutorials/copilot-deploy-flow/add-your-data-source-details.png#lightbox)

5. Select or create an Azure AI Search resource named *contoso-outdoor-search* and select the acknowledgment that connecting it incurs usage on your account.

    Note

   You use the *product-info* index and the *contoso-outdoor-search* Azure AI Search resource in prompt flow later in this tutorial. If the names you enter differ from what's specified here, make sure to use the names you entered in the rest of the tutorial.

6. Select the Azure subscription that contains the Azure OpenAI resource you want to use. Then select **Next**.

7. On the **Upload files** page, select **Browse for a file** and select the files you want to upload. Select the product info files that you downloaded or created earlier. See the [prerequisites](https://learn.microsoft.com/en-us/azure/ai-studio/tutorials/deploy-copilot-ai-studio#prerequisites). If you want to upload more than one file, do so now. You can't add more files later in the same playground session.

8. Select **Upload** to upload the file to your Azure Blob storage account. Then select **Next** from the bottom of the page.

   [![Screenshot of the dialog to select and upload files.](https://learn.microsoft.com/en-us/azure/ai-studio/media/tutorials/copilot-deploy-flow/add-your-data-uploaded-product-info.png)](https://learn.microsoft.com/en-us/azure/ai-studio/media/tutorials/copilot-deploy-flow/add-your-data-uploaded-product-info.png#lightbox)

9. On the **Data management** page under **Search type**, select **Keyword**. This setting helps determine how the model responds to requests. Then select **Next**.

    Note

   If you had added vector search on the **Select or add data source** page, then more options would be available here for an additional cost. For more information, see [Azure OpenAI on your data](https://learn.microsoft.com/en-us/azure/ai-services/openai/concepts/use-your-data).

10. Review the details you entered, and select **Save and close**. You can now chat with the model and it uses information from your data to construct the response.

    [![Screenshot of the review and finish page for adding data.](https://learn.microsoft.com/en-us/azure/ai-studio/media/tutorials/copilot-deploy-flow/add-your-data-review-finish.png)](https://learn.microsoft.com/en-us/azure/ai-studio/media/tutorials/copilot-deploy-flow/add-your-data-review-finish.png#lightbox)

11. Now on the **Assistant setup** pane, you can see that your data ingestion is in progress. Before proceeding, wait until you see the data source and index name in place of the status.

    [![Screenshot of the chat playground with the status of data ingestion in view.](https://learn.microsoft.com/en-us/azure/ai-studio/media/tutorials/copilot-deploy-flow/add-your-data-ingestion-in-progress.png)](https://learn.microsoft.com/en-us/azure/ai-studio/media/tutorials/copilot-deploy-flow/add-your-data-ingestion-in-progress.png#lightbox)

12. You can now chat with the model asking the same question as before ("How much do the TrailWalker hiking shoes cost"), and this time it uses information from your data to construct the response. You can expand the **references** button to see the data that was used.

    [![Screenshot of the assistant's reply with grounding data.](https://learn.microsoft.com/en-us/azure/ai-studio/media/tutorials/copilot-deploy-flow/chat-with-data.png)](https://learn.microsoft.com/en-us/azure/ai-studio/media/tutorials/copilot-deploy-flow/chat-with-data.png#lightbox)



## Create compute and runtime that are needed for prompt flow

You use prompt flow to optimize the messages that are sent to the copilot's chat model. Prompt flow requires a compute instance and a runtime. If you already have a compute instance and a runtime, you can skip this section and remain in the playground.

To create a compute instance and a runtime, follow these steps:

1. If you don't have a compute instance, you can [create one in Azure AI Studio](https://learn.microsoft.com/en-us/azure/ai-studio/how-to/create-manage-compute).
2. Then create a runtime by following the steps in [how to create a runtime](https://learn.microsoft.com/en-us/azure/ai-studio/how-to/create-manage-runtime).

To complete the rest of the tutorial, make sure that your runtime is in the **Running** status. You might need to select **Refresh** to see the updated status.

 Important

You're charged for compute instances while they are running. To avoid incurring unnecessary Azure costs, pause the compute instance when you're not actively working in prompt flow. For more information, see [how to start and stop compute](https://learn.microsoft.com/en-us/azure/ai-studio/how-to/create-manage-compute#start-or-stop-a-compute-instance).



## Create a prompt flow from the playground

Now that your [deployed chat model](https://learn.microsoft.com/en-us/azure/ai-studio/tutorials/deploy-copilot-ai-studio#deploy-a-chat-model) is working in the playground [with your data](https://learn.microsoft.com/en-us/azure/ai-studio/tutorials/deploy-copilot-ai-studio#add-your-data-and-try-the-chat-model-again), you could [deploy your copilot as a web app](https://learn.microsoft.com/en-us/azure/ai-studio/tutorials/deploy-chat-web-app#deploy-your-web-app) from the playground.

But you might ask "How can I further customize this copilot?" You might want to add multiple data sources, compare different prompts or the performance of multiple models. A [prompt flow](https://learn.microsoft.com/en-us/azure/ai-studio/how-to/prompt-flow) serves as an executable workflow that streamlines the development of your LLM-based AI application. It provides a comprehensive framework for managing data flow and processing within your application.

In this section, you learn how to transition to prompt flow from the playground. You export the playground chat environment including connections to the data that you added. Later in this tutorial, you [evaluate the flow](https://learn.microsoft.com/en-us/azure/ai-studio/tutorials/deploy-copilot-ai-studio#evaluate-the-flow-using-a-question-and-answer-evaluation-dataset) and then [deploy the flow](https://learn.microsoft.com/en-us/azure/ai-studio/tutorials/deploy-copilot-ai-studio#deploy-the-flow) for [consumption](https://learn.microsoft.com/en-us/azure/ai-studio/tutorials/deploy-copilot-ai-studio#use-the-deployed-flow).

 Note

The changes made in prompt flow aren't applied backwards to update the playground environment.

You can create a prompt flow from the playground by following these steps:

1. If you aren't already in the [Azure AI Studio](https://ai.azure.com/) playground, select **Build** from the top menu and then select **Playground** from the collapsible left menu.

2. Select **Open in prompt flow** from the menu above the **Chat session** pane.

3. Enter a folder name for your prompt flow. Then select **Open**. Azure AI Studio exports the playground chat environment including connections to your data to prompt flow.

   [![Screenshot of the open in prompt flow dialog.](https://learn.microsoft.com/en-us/azure/ai-studio/media/tutorials/copilot-deploy-flow/prompt-flow-from-playground.png)](https://learn.microsoft.com/en-us/azure/ai-studio/media/tutorials/copilot-deploy-flow/prompt-flow-from-playground.png#lightbox)

Within a flow, nodes take center stage, representing specific tools with unique capabilities. These nodes handle data processing, task execution, and algorithmic operations, with inputs and outputs. By connecting nodes, you establish a seamless chain of operations that guides the flow of data through your application. For more information, see [prompt flow tools](https://learn.microsoft.com/en-us/azure/ai-studio/how-to/prompt-flow#prompt-flow-tools).

To facilitate node configuration and fine-tuning, a visual representation of the workflow structure is provided through a DAG (Directed Acyclic Graph) graph. This graph showcases the connectivity and dependencies between nodes, providing a clear overview of the entire workflow. The nodes in the graph shown here are representative of the playground chat experience that you exported to prompt flow.

[![Screenshot of the default graph exported from the playground to prompt flow.](https://learn.microsoft.com/en-us/azure/ai-studio/media/tutorials/copilot-deploy-flow/prompt-flow-overview-graph.png)](https://learn.microsoft.com/en-us/azure/ai-studio/media/tutorials/copilot-deploy-flow/prompt-flow-overview-graph.png#lightbox)

Nodes can be added, updated, rearranged, or removed. The nodes in your flow at this point include:

- **DetermineIntent**: This node determines the intent of the user's query. It uses the system prompt to determine the intent. You can edit the system prompt to provide scenario-specific few-shot examples.

- **ExtractIntent**: This node formats the output of the **DetermineIntent** node and sends it to the **RetrieveDocuments** node.

- **RetrieveDocuments**: This node searches for top documents related to the query. This node uses the search type and any parameters you pre-configured in playground.

- **FormatRetrievedDocuments**: This node formats the output of the **RetrieveDocuments** node and sends it to the **DetermineReply** node.

- DetermineReply

  : This node contains an extensive system prompt, which asks the LLM to respond using the retrieved documents only. There are two inputs:

  - The **RetrieveDocuments** node provides the top retrieved documents.
  - The **FormatConversation** node provides the formatted conversation history including the latest query.

The **FormatReply** node formats the output of the **DetermineReply** node.

In prompt flow, you should also see:

- **Save**: You can save your prompt flow at any time by selecting **Save** from the top menu. Be sure to save your prompt flow periodically as you make changes in this tutorial.

- **Runtime**: The runtime that you created [earlier in this tutorial](https://learn.microsoft.com/en-us/azure/ai-studio/tutorials/deploy-copilot-ai-studio#create-compute-and-runtime-that-are-needed-for-prompt-flow). You can start and stop runtimes and compute instances via **Settings** in the left menu. To work in prompt flow, make sure that your runtime is in the **Running** status.

  [![Screenshot of the prompt flow editor and surrounding menus.](https://learn.microsoft.com/en-us/azure/ai-studio/media/tutorials/copilot-deploy-flow/prompt-flow-overview.png)](https://learn.microsoft.com/en-us/azure/ai-studio/media/tutorials/copilot-deploy-flow/prompt-flow-overview.png#lightbox)

- **Tools**: You can return to the prompt flow anytime by selecting **Prompt flow** from **Tools** in the left menu. Then select the prompt flow folder that you created earlier (not the sample flow).

  [![Screenshot of the list of your prompt flows.](https://learn.microsoft.com/en-us/azure/ai-studio/media/tutorials/copilot-deploy-flow/prompt-flow-return.png)](https://learn.microsoft.com/en-us/azure/ai-studio/media/tutorials/copilot-deploy-flow/prompt-flow-return.png#lightbox)



## Customize prompt flow with multiple data sources

Earlier in the [Azure AI Studio](https://ai.azure.com/) playground, you [added your data](https://learn.microsoft.com/en-us/azure/ai-studio/tutorials/deploy-copilot-ai-studio#add-your-data-and-try-the-chat-model-again) to create one search index that contained product data for the Contoso copilot. So far, users can only inquire about products with questions such as "How much do the TrailWalker hiking shoes cost?". But they can't get answers to questions such as "How many TrailWalker hiking shoes did Daniel Wilson buy?" To enable this scenario, we add another index with customer information to the flow.



### Create the customer info index

You need a local copy of example customer information. For more information and links to example data, see the [prerequisites](https://learn.microsoft.com/en-us/azure/ai-studio/tutorials/deploy-copilot-ai-studio#prerequisites).

Follow these instructions on how to create a new index:

1. Select **Index** from the left menu. Then select **+ New index**.

   [![Screenshot of the indexes page with the button to create a new index.](https://learn.microsoft.com/en-us/azure/ai-studio/media/tutorials/copilot-deploy-flow/add-index-new.png)](https://learn.microsoft.com/en-us/azure/ai-studio/media/tutorials/copilot-deploy-flow/add-index-new.png#lightbox)

   You're taken to the **Create an index** wizard.

2. On the Source data page, select **Upload folder** from the **Upload** dropdown. Select the customer info files that you downloaded or created earlier. See the [prerequisites](https://learn.microsoft.com/en-us/azure/ai-studio/tutorials/deploy-copilot-ai-studio#prerequisites).

   [![Screenshot of the customer data source selection options.](https://learn.microsoft.com/en-us/azure/ai-studio/media/tutorials/copilot-deploy-flow/add-index-dataset-upload-folder.png)](https://learn.microsoft.com/en-us/azure/ai-studio/media/tutorials/copilot-deploy-flow/add-index-dataset-upload-folder.png#lightbox)

3. Select **Next** at the bottom of the page.

4. Select the same Azure AI Search resource (*contoso-outdoor-search*) that you used for your product info index (*product-info*). Then select **Next**.

   [![Screenshot of the selected Azure AI Search resource.](https://learn.microsoft.com/en-us/azure/ai-studio/media/tutorials/copilot-deploy-flow/add-index-storage.png)](https://learn.microsoft.com/en-us/azure/ai-studio/media/tutorials/copilot-deploy-flow/add-index-storage.png#lightbox)

5. Select **Hybrid + Semantic (Recommended)** for the **Search type**. This type should be selected by default.

6. Select *Default_AzureOpenAI* from the **Azure OpenAI resource** dropdown. Select the checkbox to acknowledge that an Azure OpenAI embedding model will be deployed if it's not already. Then select **Next**.

   [![Screenshot of index search type options.](https://learn.microsoft.com/en-us/azure/ai-studio/media/tutorials/copilot-deploy-flow/add-index-search-settings.png)](https://learn.microsoft.com/en-us/azure/ai-studio/media/tutorials/copilot-deploy-flow/add-index-search-settings.png#lightbox)

    Note

   The embedding model is listed with other model deployments in the **Deployments** page.

7. Enter **customer-info** for the index name. Then select **Next**.

   [![Screenshot of the index name and virtual machine options.](https://learn.microsoft.com/en-us/azure/ai-studio/media/tutorials/copilot-deploy-flow/add-index-settings.png)](https://learn.microsoft.com/en-us/azure/ai-studio/media/tutorials/copilot-deploy-flow/add-index-settings.png#lightbox)

8. Review the details you entered, and select **Create**.

   [![Screenshot of the review and finish index creation page.](https://learn.microsoft.com/en-us/azure/ai-studio/media/tutorials/copilot-deploy-flow/add-index-review.png)](https://learn.microsoft.com/en-us/azure/ai-studio/media/tutorials/copilot-deploy-flow/add-index-review.png#lightbox)

    Note

   You use the *customer-info* index and the *contoso-outdoor-search* Azure AI Search resource in prompt flow later in this tutorial. If the names you enter differ from what's specified here, make sure to use the names you entered in the rest of the tutorial.

9. You're taken to the index details page where you can see the status of your index creation

   [![Screenshot of the customer info index details.](https://learn.microsoft.com/en-us/azure/ai-studio/media/tutorials/copilot-deploy-flow/add-index-created-details.png)](https://learn.microsoft.com/en-us/azure/ai-studio/media/tutorials/copilot-deploy-flow/add-index-created-details.png#lightbox)

For more information on how to create an index, see [Create an index](https://learn.microsoft.com/en-us/azure/ai-studio/how-to/index-add).



### Add customer information to the flow

After you're done creating your index, return to your prompt flow and follow these steps to add the customer info to the flow:

1. Select the **RetrieveDocuments** node from the graph and rename it **RetrieveProductInfo**. Now the retrieve product info node can be distinguished from the retrieve customer info node that you add to the flow.

   [![Screenshot of the prompt flow node for retrieving product info.](https://learn.microsoft.com/en-us/azure/ai-studio/media/tutorials/copilot-deploy-flow/node-rename-retrieve-product-info.png)](https://learn.microsoft.com/en-us/azure/ai-studio/media/tutorials/copilot-deploy-flow/node-rename-retrieve-product-info.png#lightbox)

2. Select **+ Python** from the top menu to create a new [Python node](https://learn.microsoft.com/en-us/azure/ai-studio/how-to/prompt-flow-tools/python-tool) that's used to retrieve customer information.

   [![Screenshot of the prompt flow node for retrieving customer info.](https://learn.microsoft.com/en-us/azure/ai-studio/media/tutorials/copilot-deploy-flow/node-new-retrieve-customer-info.png)](https://learn.microsoft.com/en-us/azure/ai-studio/media/tutorials/copilot-deploy-flow/node-new-retrieve-customer-info.png#lightbox)

3. Name the node **RetrieveCustomerInfo** and select **Add**.

4. Copy and paste the Python code from the **RetrieveProductInfo** node into the **RetrieveCustomerInfo** node to replace all of the default code.

5. Select the **Validate and parse input** button to validate the inputs for the **RetrieveCustomerInfo** node. If the inputs are valid, prompt flow parses the inputs and creates the necessary variables for you to use in your code.

   [![Screenshot of the validate and parse input button.](https://learn.microsoft.com/en-us/azure/ai-studio/media/tutorials/copilot-deploy-flow/customer-info-validate-parse.png)](https://learn.microsoft.com/en-us/azure/ai-studio/media/tutorials/copilot-deploy-flow/customer-info-validate-parse.png#lightbox)

6. Edit the **RetrieveCustomerInfo** inputs that prompt flow parsed for you so that it can connect to your *customer-info* index.

   [![Screenshot of inputs to edit in the retrieve customer info node.](https://learn.microsoft.com/en-us/azure/ai-studio/media/tutorials/copilot-deploy-flow/customer-info-edit-inputs.png)](https://learn.microsoft.com/en-us/azure/ai-studio/media/tutorials/copilot-deploy-flow/customer-info-edit-inputs.png#lightbox)

    Note

   The graph is updated immediately after you set the **queries** input value to **ExtractIntent.output.search_intents**. In the graph you can see that **RetrieveCustomerInfo** gets inputs from **ExtractIntent**.

   The inputs are case sensitive, so be sure they match these values exactly:

   Expand table

   | Name                         | Type             | Value                                    |
   | :--------------------------- | :--------------- | :--------------------------------------- |
   | **embeddingModelConnection** | Azure OpenAI     | *Default_AzureOpenAI*                    |
   | **embeddingModelName**       | string           | *None*                                   |
   | **indexName**                | string           | *customer-info*                          |
   | **queries**                  | string           | *${ExtractIntent.output.search_intents}* |
   | **queryType**                | string           | *simple*                                 |
   | **searchConnection**         | Cognitive search | *contoso-outdoor-search*                 |
   | **semanticConfiguration**    | string           | *None*                                   |
   | **topK**                     | int              | *5*                                      |

7. Select **Save** from the top menu to save your changes.



### Format the retrieved documents to output

Now that you have both the product and customer info in your prompt flow, you format the retrieved documents so that the large language model can use them.

1. Select the **FormatRetrievedDocuments** node from the graph.

2. Copy and paste the following Python code to replace all contents in the **FormatRetrievedDocuments** code block.

   Python

   ```python
   from promptflow import tool
   
   @tool
   def format_retrieved_documents(docs1: object, docs2: object, maxTokens: int) -> str:
     formattedDocs = []
     strResult = ""
     docs = [val for pair in zip(docs1, docs2) for val in pair]
     for index, doc in enumerate(docs):
       formattedDocs.append({
         f"[doc{index}]": {
           "title": doc['title'],
           "content": doc['content']
         }
       })
       formattedResult = { "retrieved_documents": formattedDocs }
       nextStrResult = str(formattedResult)
       if (estimate_tokens(nextStrResult) > maxTokens):
         break
       strResult = nextStrResult
   
     return {
             "combined_docs": docs,
             "strResult": strResult
         }
   
   def estimate_tokens(text: str) -> int:
     return (len(text) + 2) / 3
   ```

3. Select the **Validate and parse input** button to validate the inputs for the **FormatRetrievedDocuments** node. If the inputs are valid, prompt flow parses the inputs and creates the necessary variables for you to use in your code.

4. Edit the **FormatRetrievedDocuments** inputs that prompt flow parsed for you so that it can extract product and customer info from the **RetrieveProductInfo** and **RetrieveCustomerInfo** nodes.

   [![Screenshot of inputs to edit in the format retrieved documents node.](https://learn.microsoft.com/en-us/azure/ai-studio/media/tutorials/copilot-deploy-flow/format-retrieved-documents-edit-inputs.png)](https://learn.microsoft.com/en-us/azure/ai-studio/media/tutorials/copilot-deploy-flow/format-retrieved-documents-edit-inputs.png#lightbox)

   The inputs are case sensitive, so be sure they match these values exactly:

   Expand table

   | Name          | Type   | Value                            |
   | :------------ | :----- | :------------------------------- |
   | **docs1**     | object | *${RetrieveProductInfo.output}*  |
   | **docs2**     | object | *${RetrieveCustomerInfo.output}* |
   | **maxTokens** | int    | *5000*                           |

5. Select the **DetermineReply** node from the graph.

6. Set the **documentation** input to *${FormatRetrievedDocuments.output.strResult}*.

   [![Screenshot of editing the documentation input value in the determine reply node.](https://learn.microsoft.com/en-us/azure/ai-studio/media/tutorials/copilot-deploy-flow/determine-reply-edit-inputs.png)](https://learn.microsoft.com/en-us/azure/ai-studio/media/tutorials/copilot-deploy-flow/determine-reply-edit-inputs.png#lightbox)

7. Select the **outputs** node from the graph.

8. Set the **fetched_docs** input to *${FormatRetrievedDocuments.output.combined_docs}*.

   [![Screenshot of editing the fetched_docs input value in the outputs node.](https://learn.microsoft.com/en-us/azure/ai-studio/media/tutorials/copilot-deploy-flow/outputs-edit.png)](https://learn.microsoft.com/en-us/azure/ai-studio/media/tutorials/copilot-deploy-flow/outputs-edit.png#lightbox)

9. Select **Save** from the top menu to save your changes.



### Chat in prompt flow with product and customer info

By now you have both the product and customer info in prompt flow. You can chat with the model in prompt flow and get answers to questions such as "How many TrailWalker hiking shoes did Daniel Wilson buy?" Before proceeding to a more formal evaluation, you can optionally chat with the model to see how it responds to your questions.

1. Select **Chat** from the top menu in prompt flow to try chat.

2. Enter "How many TrailWalker hiking shoes did Daniel Wilson buy?" and then select the right arrow icon to send.

3. The response is what you expect. The model uses the customer info to answer the question.

   [![Screenshot of the assistant's reply with product and customer grounding data.](https://learn.microsoft.com/en-us/azure/ai-studio/media/tutorials/copilot-deploy-flow/chat-with-data-customer.png)](https://learn.microsoft.com/en-us/azure/ai-studio/media/tutorials/copilot-deploy-flow/chat-with-data-customer.png#lightbox)



## Evaluate the flow using a question and answer evaluation dataset

In [Azure AI Studio](https://ai.azure.com/), you want to evaluate the flow before you [deploy the flow](https://learn.microsoft.com/en-us/azure/ai-studio/tutorials/deploy-copilot-ai-studio#deploy-the-flow) for [consumption](https://learn.microsoft.com/en-us/azure/ai-studio/tutorials/deploy-copilot-ai-studio#use-the-deployed-flow).

In this section, you use the built-in evaluation to evaluate your flow with a question and answer evaluation dataset. The built-in evaluation uses AI-assisted metrics to evaluate your flow: groundedness, relevance, and retrieval score. For more information, see [built-in evaluation metrics](https://learn.microsoft.com/en-us/azure/ai-studio/concepts/evaluation-metrics-built-in).



### Create an evaluation

You need a question and answer evaluation dataset that contains questions and answers that are relevant to your scenario. Create a new file locally named **qa-evaluation.jsonl**. Copy and paste the following questions and answers (`"truth"`) into the file.

JSON

```json
{"question": "What color is the CozyNights Sleeping Bag?", "truth": "Red", "chat_history": []}
{"question": "When did Daniel Wilson order the BaseCamp Folding Table?", "truth": "May 7th, 2023", "chat_history": []}
{"question": "How much do TrailWalker Hiking Shoes cost? ", "truth": "$110", "chat_history": []}
{"question": "What kind of tent did Sarah Lee buy?", "truth": "SkyView 2 person tent", "chat_history": []}
{"question": "What is Melissa Davis's phone number?", "truth": "555-333-4444", "chat_history": []}
{"question": "What is the proper care for trailwalker hiking shoes?", "truth": "After each use, remove any dirt or debris by brushing or wiping the shoes with a damp cloth.", "chat_history": []}
{"question": "Does TrailMaster Tent come with a warranty?", "truth": "2 years", "chat_history": []}
{"question": "How much did David Kim spend on the TrailLite Daypack?", "truth": "$240", "chat_history": []}
{"question": "What items did Amanda Perez purchase?", "truth": "TrailMaster X4 Tent, TrekReady Hiking Boots (quantity 3), CozyNights Sleeping Bag, TrailBlaze Hiking Pants, RainGuard Hiking Jacket, and CompactCook Camping Stove", "chat_history": []}
{"question": "What is the Brand for TrekReady Hiking Boots", "truth": "TrekReady", "chat_history": []}
{"question": "How many items did Karen Williams buy?", "truth": "three items of the Summit Breeze Jacket", "chat_history": []}
{"question": "France is in Europe", "truth": "Sorry, I can only truth questions related to outdoor/camping gear and equipment", "chat_history": []}
```

Now that you have your evaluation dataset, you can evaluate your flow by following these steps:

1. Select **Evaluate** > **Built-in evaluation** from the top menu in prompt flow.

   [![Screenshot of the option to create a built-in evaluation from prompt flow.](https://learn.microsoft.com/en-us/azure/ai-studio/media/tutorials/copilot-deploy-flow/evaluate-built-in-evaluation.png)](https://learn.microsoft.com/en-us/azure/ai-studio/media/tutorials/copilot-deploy-flow/evaluate-built-in-evaluation.png#lightbox)

   You're taken to the **Create a new evaluation** wizard.

2. Enter a name for your evaluation and select a runtime.

3. Select **Question and answer pairs with retrieval-augmented generation** from the scenario options.

   [![Screenshot of selecting an evaluation scenario.](https://learn.microsoft.com/en-us/azure/ai-studio/media/tutorials/copilot-deploy-flow/evaluate-basic-scenario.png)](https://learn.microsoft.com/en-us/azure/ai-studio/media/tutorials/copilot-deploy-flow/evaluate-basic-scenario.png#lightbox)

4. Select the flow to evaluate. In this example, select *Contoso outdoor flow* or whatever you named your flow. Then select **Next**.

5. Select the metrics you want to use to evaluate your flow. In this example, select **Groundedness**, **Relevance**, and **Retrieval score**.

   [![Screenshot of selecting evaluation metrics.](https://learn.microsoft.com/en-us/azure/ai-studio/media/tutorials/copilot-deploy-flow/evaluate-metrics.png)](https://learn.microsoft.com/en-us/azure/ai-studio/media/tutorials/copilot-deploy-flow/evaluate-metrics.png#lightbox)

6. Select a model to use for evaluation. In this example, select **gpt-35-turbo-16k**. Then select **Next**.

    Note

   Evaluation with AI-assisted metrics needs to call another GPT model to do the calculation. For best performance, use a GPT-4 or gpt-35-turbo-16k model. If you didn't previously deploy a GPT-4 or gpt-35-turbo-16k model, you can deploy another model by following the steps in [Deploy a chat model](https://learn.microsoft.com/en-us/azure/ai-studio/tutorials/deploy-copilot-ai-studio#deploy-a-chat-model). Then return to this step and select the model you deployed.

7. Select **Add new dataset**. Then select **Next**.

   [![Screenshot of the option to use a new or existing dataset.](https://learn.microsoft.com/en-us/azure/ai-studio/media/tutorials/copilot-deploy-flow/evaluate-add-dataset.png)](https://learn.microsoft.com/en-us/azure/ai-studio/media/tutorials/copilot-deploy-flow/evaluate-add-dataset.png#lightbox)

8. Select **Upload files**, browse files, and select the **qa-evaluation.jsonl** file that you created earlier.

   [![Screenshot of the dataset upload files button.](https://learn.microsoft.com/en-us/azure/ai-studio/media/tutorials/copilot-deploy-flow/evaluate-upload-files.png)](https://learn.microsoft.com/en-us/azure/ai-studio/media/tutorials/copilot-deploy-flow/evaluate-upload-files.png#lightbox)

9. After the file is uploaded, you need to map the properties from the file (data source) to the evaluation properties. Enter the following values for each data source property:

   [![Screenshot of the evaluation dataset mapping.](https://learn.microsoft.com/en-us/azure/ai-studio/media/tutorials/copilot-deploy-flow/evaluate-map-data-source.png)](https://learn.microsoft.com/en-us/azure/ai-studio/media/tutorials/copilot-deploy-flow/evaluate-map-data-source.png#lightbox)

   Expand table

   | Name             | Description                                               | Type   | Data source                 |
   | :--------------- | :-------------------------------------------------------- | :----- | :-------------------------- |
   | **chat_history** | The chat history                                          | list   | *${data.chat_history}*      |
   | **query**        | The query                                                 | string | *${data.question}*          |
   | **question**     | A query seeking specific information                      | string | *${data.question}*          |
   | **answer**       | The response to question generated by the model as answer | string | ${run.outputs.reply}        |
   | **documents**    | String with context from retrieved documents              | string | ${run.outputs.fetched_docs} |

10. Select **Next**.

11. Review the evaluation details and then select **Submit**.

    [![Screenshot of the review and finish page within the create evaluation dialog.](https://learn.microsoft.com/en-us/azure/ai-studio/media/tutorials/copilot-deploy-flow/evaluate-review-finish.png)](https://learn.microsoft.com/en-us/azure/ai-studio/media/tutorials/copilot-deploy-flow/evaluate-review-finish.png#lightbox)

    You're taken to the **Metric evaluations** page.



### View the evaluation status and results

Now you can view the evaluation status and results by following these steps:

1. After you [create an evaluation](https://learn.microsoft.com/en-us/azure/ai-studio/tutorials/deploy-copilot-ai-studio#create-an-evaluation), if you aren't there already go to **Build** > **Evaluation**. On the **Metric evaluations** page, you can see the evaluation status and the metrics that you selected. You might need to select **Refresh** after a couple of minutes to see the **Completed** status.

   [![Screenshot of the metric evaluations page.](https://learn.microsoft.com/en-us/azure/ai-studio/media/tutorials/copilot-deploy-flow/evaluate-status-completed.png)](https://learn.microsoft.com/en-us/azure/ai-studio/media/tutorials/copilot-deploy-flow/evaluate-status-completed.png#lightbox)

    Tip

   Once the evaluation is in **Completed** status, you don't need runtime or compute to complete the rest of this tutorial. You can stop your compute instance to avoid incurring unnecessary Azure costs. For more information, see [how to start and stop compute](https://learn.microsoft.com/en-us/azure/ai-studio/how-to/create-manage-compute#start-or-stop-a-compute-instance).

2. Select the name of the evaluation that completed first (*contoso-evaluate-from-flow_variant_0*) to see the evaluation details with the columns that you mapped earlier.

   [![Screenshot of the detailed metrics results page.](https://learn.microsoft.com/en-us/azure/ai-studio/media/tutorials/copilot-deploy-flow/evaluate-view-results-detailed.png)](https://learn.microsoft.com/en-us/azure/ai-studio/media/tutorials/copilot-deploy-flow/evaluate-view-results-detailed.png#lightbox)

3. Select the name of the evaluation that completed second (*evaluation_contoso-evaluate-from-flow_variant_0*) to see the evaluation metrics: **Groundedness**, **Relevance**, and **Retrieval score**.

   [![Screenshot of the average metrics scores.](https://learn.microsoft.com/en-us/azure/ai-studio/media/tutorials/copilot-deploy-flow/evaluate-view-results-metrics.png)](https://learn.microsoft.com/en-us/azure/ai-studio/media/tutorials/copilot-deploy-flow/evaluate-view-results-metrics.png#lightbox)

For more information, see [view evaluation results](https://learn.microsoft.com/en-us/azure/ai-studio/how-to/evaluate-flow-results).



## Deploy the flow

Now that you [built a flow](https://learn.microsoft.com/en-us/azure/ai-studio/tutorials/deploy-copilot-ai-studio#create-a-prompt-flow-from-the-playground) and completed a metrics-based [evaluation](https://learn.microsoft.com/en-us/azure/ai-studio/tutorials/deploy-copilot-ai-studio#evaluate-the-flow-using-a-question-and-answer-evaluation-dataset), it's time to create your online endpoint for real-time inference. That means you can use the deployed flow to answer questions in real time.

Follow these steps to deploy a prompt flow as an online endpoint from [Azure AI Studio](https://ai.azure.com/).

1. Have a prompt flow ready for deployment. If you don't have one, see [how to build a prompt flow](https://learn.microsoft.com/en-us/azure/ai-studio/how-to/flow-develop).

2. Optional: Select **Chat** to test if the flow is working correctly. Testing your flow before deployment is recommended best practice.

3. Select **Deploy** on the flow editor.

   [![Screenshot of the deploy button from a prompt flow editor.](https://learn.microsoft.com/en-us/azure/ai-studio/media/tutorials/copilot-deploy-flow/deploy-from-flow.png)](https://learn.microsoft.com/en-us/azure/ai-studio/media/tutorials/copilot-deploy-flow/deploy-from-flow.png#lightbox)

4. Provide the requested information on the **Basic Settings** page in the deployment wizard.

   [![Screenshot of the basic settings page in the deployment wizard.](https://learn.microsoft.com/en-us/azure/ai-studio/media/tutorials/copilot-deploy-flow/deploy-basic-settings.png)](https://learn.microsoft.com/en-us/azure/ai-studio/media/tutorials/copilot-deploy-flow/deploy-basic-settings.png#lightbox)

5. Select **Next** to proceed to the advanced settings pages.

6. On the **Advanced settings - Endpoint** page, leave the default settings and select **Next**.

7. On the **Advanced settings - Deployment** page, leave the default settings and select **Next**.

8. On the **Advanced settings - Outputs & connections** page, make sure all outputs are selected under **Included in endpoint response**.

   [![Screenshot of the advanced settings page in the deployment wizard.](https://learn.microsoft.com/en-us/azure/ai-studio/media/tutorials/copilot-deploy-flow/deploy-advanced-outputs-connections.png)](https://learn.microsoft.com/en-us/azure/ai-studio/media/tutorials/copilot-deploy-flow/deploy-advanced-outputs-connections.png#lightbox)

9. Select **Review + Create** to review the settings and create the deployment.

10. Select **Create** to deploy the prompt flow.

    [![Screenshot of the review prompt flow deployment settings page.](https://learn.microsoft.com/en-us/azure/ai-studio/media/tutorials/copilot-deploy-flow/deploy-review-create.png)](https://learn.microsoft.com/en-us/azure/ai-studio/media/tutorials/copilot-deploy-flow/deploy-review-create.png#lightbox)

For more information, see [how to deploy a flow](https://learn.microsoft.com/en-us/azure/ai-studio/how-to/flow-deploy).



## Use the deployed flow

Your copilot application can use the deployed prompt flow to answer questions in real time. You can use the REST endpoint or the SDK to use the deployed flow.

1. To view the status of your deployment in [Azure AI Studio](https://ai.azure.com/), select **Deployments** from the left navigation. Once the deployment is created successfully, you can select the deployment to view the details.

   [![Screenshot of the prompt flow deployment state in progress.](https://learn.microsoft.com/en-us/azure/ai-studio/media/tutorials/copilot-deploy-flow/deployments-state-updating.png)](https://learn.microsoft.com/en-us/azure/ai-studio/media/tutorials/copilot-deploy-flow/deployments-state-updating.png#lightbox)

    Note

   If you see a message that says "Currently this endpoint has no deployments" or the **State** is still *Updating*, you might need to select **Refresh** after a couple of minutes to see the deployment.

2. Optionally, the details page is where you can change the authentication type or enable monitoring.

   [![Screenshot of the prompt flow deployment details page.](https://learn.microsoft.com/en-us/azure/ai-studio/media/tutorials/copilot-deploy-flow/deploy-authentication-monitoring.png)](https://learn.microsoft.com/en-us/azure/ai-studio/media/tutorials/copilot-deploy-flow/deploy-authentication-monitoring.png#lightbox)

3. Select the **Consume** tab. You can see code samples and the REST endpoint for your copilot application to use the deployed flow.

   [![Screenshot of the prompt flow deployment endpoint and code samples.](https://learn.microsoft.com/en-us/azure/ai-studio/media/tutorials/copilot-deploy-flow/deployments-score-url-samples.png)](https://learn.microsoft.com/en-us/azure/ai-studio/media/tutorials/copilot-deploy-flow/deployments-score-url-samples.png#lightbox)


