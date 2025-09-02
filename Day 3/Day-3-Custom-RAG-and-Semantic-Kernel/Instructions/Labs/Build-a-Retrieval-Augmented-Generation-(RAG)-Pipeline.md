!INSTRUCTIONS [ ](https://raw.githubusercontent.com/LODSContent/All-MOC/master/MOC/@lab.LanguageCode/MSDepth_GenericLogin.md)


===


# Exercise 1: Set Up Azure AI Foundry SDK and Provision Resources 

In this exercise, you will set up the Azure AI Foundry SDK. This includes configuring the environment, deploying foundation models, and ensuring seamless integration with Azure AI services for knowledge retrieval and inference.

## Setting up the Prerequisite Resources


1. On the Azure Portal page +++https://portal.azure.com+++, in the Search resources box at the top of the portal, enter Azure AI Foundry, and then select Azure AI Foundry under Services.


2. In the left navigation pane for the AI Foundry, select **AI Hubs**. On the AI Hubs page, click on **Create** and select **Hub** from the drop-down.


3. On the **Create an Azure AI hub** pane enter the following details:
   - Subscription : **Leave default subscription**
   - Resource Group :  **AgenticAI**
   - Region : **EastUS**
   - Name : +++ai-foundry-hub@lab.LabInstance.Id+++ 
   - Connect AI Services incl. OpenAI : Click on **Create New**
   - Connect AI Services incl. OpenAI : Provide a name +++my-ai-service@lab.LabInstance.Id+++  
   - Click on **Save**, followed by **Next:Storage**

   
4. Click on **Review + Create** tab followed by **Create.**
  
5. Wait for the deployment is completed and then click on **Go to resource**.

6. On the Overview pane, click on **Launch Azure AI Foundry**. This will navigate you to the Azure AI Foundry portal..

7. Select **+ New project** on the Hub Overview.

8. Provide the project name as +++ai-foundry-project@lab.LabInstance.Id+++ then select **Create**.

1. In your **AI Foundry project**, navigate to the **My assets** section, then select **Models + endpoints**. Click **Deploy model**, and choose **Deploy base model** to proceed.

1. On a **Select a model** window, search for **gpt-4o**, select **gpt-4o** and select **Confirm**


1. On **Deploy model gpt-4o** window, select **Customize**.


      - Deployment Name: **gpt-4o**
      - Deployment type: **Global Standard**
      - Change the **Model version to 2024-08-06 (Default)**
      - Change the Tokens per Minute Rate Limit to **200K**
      - Click on **Deploy (5)**

1. Navigate back to **Azure Portal** and search for **AI Search** and select **AI Search** resource.


1. On the **AI Foundry | AI Search** page, select **+ Create** to create Azure OpenAI resource.


1. On **Create Azure OpenAI** page, provide the following settings and select **Next (6)**:

      | Setting | Value | 
      | --- | --- |
      | Subscription | Keep the default subscription |
      | Resource group | **AgenticAI** |
      | Region | **East US 2** |
      | Name | +++aisearch@lab.LabInstance.Id+++ |
      | Pricing tier | **Standard S0** |


1. Select **Review + create**, then **Create**

1. Back on the Azure AI Foundry tab, Select **Management Center**.

1. Under your Project select **Connected resources**.  Then select **+New connection**.  Select **Azure AI Search**, then choose **Add connection** then **Close**.

1. Next, select **Connected resources** below your Foundry hub. Then select **+New connection**.  Select **Azure AI Search**, then choose **Add connection** then **Close**.




### Task 1: Install the requirements for the Project

In this task, you will clone the GitHub repository for the project to access the necessary files for building the chat app.

1. On your **Lab VM**, launch **Visual Studio Code**.

1. Click on **File (1)**, then **Open Folder**.

1. Navigate to `C:\LabFiles\Day-3-Custom-RAG-and-Semantic-Kernel` **(1)**, select the **Custom-RAG-App (2)** folder and then click on **Select folder (3)**.

1. Click on **Yes, I trust the author**.

    ![](../media/af25.png)

1. Expand **scenarios (1)**, then **rag/custom-rag-app (2)**. Select **requirements.txt (3)**. This file contains the necessary packages for setting up Azure AI Foundry SDK. **(4)**

    ![](../media/af-27.png)

     >**Note**: This file contains the necessary packages for building and managing an AI-powered application using the Azure AI Foundry SDK, including authentication, AI inference, search, data processing, and telemetry logging.

1. Right-click on the **rag/custom-rag-app (1)** folder, then select **Open in Integrated Terminal (2)**.

    ![](../media/af26.png)

1. Install the required packages by running the following command.

    ```bash
    pip install -r requirements.txt
    ```

    ![](../media/af28.png)    

      >**Note:** Wait for the installation to complete. It might take some time.


### Task 2: Configure Environment Variables

In this task, you will set up and configure the necessary environment variables to ensure seamless integration between your RAG application and Azure AI Foundry services.

1. Open a new tab in the browser and navigate to Azure AI Foundry portal using below link

    ```
    https://ai.azure.com/
    ```

1. Click on the **Azure AI Foundry** Icon on top left.
1. Select your AI foundry project.
1. Navigate to your **Overview (1)** page of **ai-foundry-project-<inject key="Deployment ID" enableCopy="false"></inject>** and then copy and paste the **Project connection string (2)** in a notepad. You will be using it in the next step.

1. Get back to **Visual Studio Code**.

1. Right-click on **.env.sample (1)** and select **Rename (2)**.

    ![](../media/af29.png)

1. Rename the file to `.env`.

1. Click on the `.env` file and update the following values in the file

    - Replace **your_connection_string** with the **Project connection string** you copied in Step 2.
    - CHAT_MODEL="gpt-4o"
    - EVALUATION_MODEL="gpt-4o"
    - INTENT_MAPPING_MODEL="gpt-4o"

    ![](../media/focus6.png)

1. Press **Ctrl+S** to save the file.

### Review

This exercise guided participants through setting up a project in Azure AI Foundry, deploying and managing AI models, and creating an Azure AI Search service for efficient data retrieval. They integrated the search service with their project, cloned a GitHub repository containing necessary resources, and configured environment variables to ensure seamless execution.

In this exercise, you have accomplished the following:
- Task 1: Install the requirements for the Project
- Task 2: Configure Environment Variables

### You have successfully finished the exercise. Click **Next** to continue to the next exercise.

===

# Exercise 2: Build a Retrieval-Augmented Generation (RAG) Pipeline

In this exercise, you will enhance a basic chat application by integrating a Retrieval-Augmented Generation (RAG) pipeline. This includes indexing knowledge sources, implementing a retrieval mechanism, generating responses with augmented knowledge, and adding telemetry logging to monitor performance and accuracy.

## Objectives

In this exercise, you will complete the following tasks:

- Task 1: Indexing Knowledge Sources
- Task 2: Implementing the Retrieval Pipeline
- Task 3: Generating Responses with Augmented Knowledge
- Task 4: Add telemetry logging

### Task 1: Indexing Knowledge Sources 

In this task, you will index knowledge sources by processing and storing vectorized data from a CSV file using a search index. You will also authenticate your Azure account, execute the indexing script, and register the index to your cloud project.

1. Open a new tab in the browser and navigate to Azure AI Foundry portal using below link

    ```
    https://ai.azure.com/
    ```

1. Click on the **Azure AI Foundry** Icon on top left.
1. Select the AI foundry project that you created earlier in the lab i.e. **ai-foundry-project-<inject key="Deployment ID" enableCopy="false"></inject> (1)**
1. Click on **Models + endpoints (1)** under **My assets** in the left pane and then click on **+ Deploy model**, followed by **Deploy Base model (2)**.

1. Search for **text-embedding-ada-002**, select the model and click on **Confirm**.
1. Click on **Deploy**.
1. Navigate to `rag/custom-rag-app/assets` **(1)** folder and select **products.csv** file **(2)**. This file contains examples of all data sets to be used in your chat app.

    ![](../media/af35.png)

1. Select **create_search_index.py**, which stores vectorized data from the embeddings model.  

    ![](../media/af-34.png)

1. Go through the following code list as it contains:

    - Code to import the required libraries, create a project client, and configure some settings:


      *imports_and_config*


    - Code to add the function to define a search index:  


      *create_search_index*


    - Code to create the function to add a CSV file to the index:    


      *add_csv_to_index*


    - Code to run the functions, build the index, and register it to the cloud project:  


      *test_create_index*
    

1. From your console, log in to your Azure account and follow the instructions for authenticating your account:

    ```bash
    az login
    ```

    ![](../media/af36.png)

1. Minimize the Visual Studio Code window.

    - Select **Work or School account (1)** and click **Continue (2).**

    ![](../media/af37.png)    

    - Enter the **Username <inject key="AzureAdUserEmail"></inject> (1)**,  then click **Next (2).**

    ![](../media/af38.png)  

    - Enter the **Password <inject key="AzureAdUserPassword"></inject> (1)**, then click **Sign in (2).**

    ![](../media/af39.png)    

    - Click on **No, sign in to this app only.**

    ![](../media/rg10.png)      

1. Navigate back to the Visual Studio Code terminal and press **Enter** to accept the default subscription if prompted.

    ![](../media/af-41.png)

1. Back on the Azure AI Foundry tab, Select **Management Center**.

1. Under your Project select **Connected resources**.  Then select **+New connection**.  Select **Azure AI Foundry**, then choose **Add connection** then **Close**.

1. Next, select **Connected resources** below your Foundry hub. Then select **+New connection**.  Select **Azure AI Foundry**, then choose **Add connection** then **Close**.

1. Back in VS Code run the below commands to install the specfic version of Azure AI Project & Infernce:

    +++pip install azure-ai-projects==1.0.0b5+++
   

   
    +++pip install azure-ai-inference==1.0.0b8+++
   

1. Run the code to build your index locally and register it to the cloud project:


    +++python create_search_index.py+++
  

    ![](../media/af42.png)

    > **Note:** In case of an error, please run the below command and re-run the step 14:


    +++pip install --upgrade azure-search-documents+++
  


### Task 2: Implementing the Retrieval Pipeline 

In this task, you will implement the retrieval pipeline by extracting relevant product documents from the search index. You will configure and execute a script that transforms user queries into search requests, retrieving the most relevant results from the indexed knowledge source.

1. Select the **get_product_documents.py** file containing the script to get product documents from the search index.

    ![](../media/af43.png)

      - This file contains the code to import the required libraries, create a project client, and configure settings.
      - Code to add the function to get product documents.
      - Finally, add code to test the function when you run the script directly.

1. Expand **assets (1)** and select **intent_mapping.prompty (2)**. This template instructs how to extract the user's intent from the conversation.      

    ![](../media/af44.png)

      - The **get_product_documents.py** script uses this prompt template to convert the conversation to a search query.

1. Now, run the command below in the terminal to test what documents the search index returns from a query.

     ```bash
     python get_product_documents.py --query "I need a new tent for 4 people, what would you recommend?"
     ```

    ![](../media/af45.png)     

### Task 3: Generating Responses with Augmented Knowledge     

In this task, you will generate responses using augmented knowledge by leveraging retrieved product documents. You will run a script that integrates retrieval-augmented generation (RAG) capabilities to provide relevant and grounded responses based on user queries.

1. Select the **chat_with_products.py** file. This script retrieves product documents and generates a response to a user's question.

    ![](../media/af46.png)

      - This script contains code to import the required libraries, create a project client, and configure settings.   
      - Code to generate the chat function that uses the RAG capabilities.
      - Finally, add the code to run the chat function. 

1. Expand the **assets (1)** folder and select **grounded_chat.prompty (2)**. This template instructs how to generate a response based on the user's question and the retrieved documents.

    ![](../media/af47.png)

      - The **chat_with_products.py** script calls this prompt template to create a response to the user's question.

1. Run the command below in the terminal to use the script and test your chat app with RAG capabilities.

    ```bash
    python chat_with_products.py --query "I need a new tent for 4 people, what would you recommend?"
    ```

    ![](../media/af48.png)  

### Task 4: Add Telemetry Logging

In this task, you will enable telemetry logging by integrating Application Insights into your project. This allows you to monitor and analyze your RAG application's performance, track queries, and log response details for better observability and debugging.

1. Navigate back to Azure AI Foundry portal and select the AI foundry project that you created earlier in the lab i.e. **ai-foundry-project-<inject key="Deployment ID" enableCopy="false"></inject> (1)**

1. Select the **Tracing (1)** tab to add an **Application Insights** resource to your project, and then click on the **Create new (2)** option to create a new resource.

    ![](../media/af49.png)

1. Enter the name as **Applicationinsight (1)**, then click on **Create (2)**.

    ![](../media/af50.png)

1. Navigate back to the VS Code terminal and run the below command to install the *azure-monitor-opentelemetry*.

      ```bash
      pip install azure-monitor-opentelemetry
      ```

    ![](../media/af51.png)   

    > **Note:** Wait for the installation to complete. This might take some time.

1. Add the *--enable-telemetry* flag when you use the *chat_with_products.py* script:

      
      +++python chat_with_products.py --query "I need a new tent for 4 people, what would you recommend?" --enable-telemetry+++ 
           

    ![](../media/af52.png)   

1. **Ctrl+click** on the link in the console output to see the telemetry data in your Application Insights resource **(1)** and click **Open (2)**.    

    ![](../media/af53.png)

1. This will take you to the **Azure AI Foundry** portal, **Tracing** tab, where you can see the telemetry data in your Application Insights resource. 

    ![](../media/af54.png)

      > **Note:** If it does not appear immediately, select **Refresh** in the toolbar. It may take around 5 minutes to appear.

1. In your project, you can **filter** your traces as you see fit. Click on **Filter**.

    ![](../media/af55.png)

1. Click on **+ Add filter**, set the filter to **Success (1)**, **Equal to (2)** -> **True (3),** and then click on **Apply (4)**.

    ![](../media/focus7.png)

1. Now, you can only see the data with Success as **True**.

    ![](../media/af57.png)

### Review

This exercise focused on building a Retrieval-Augmented Generation (RAG) pipeline by indexing knowledge sources and implementing an efficient retrieval system. Participants generated AI responses enriched with relevant data and integrated telemetry logging to monitor and optimize system performance.

In this exercise, you have accomplished the following tasks:
- Task 1: Indexed Knowledge Sources
- Task 2: Implemented the Retrieval Pipeline
- Task 3: Generated Responses with Augmented Knowledge
- Task 4: Added telemetry logging

### You have successfully finished the exercise. Click **Next** to continue to the next exercise.    

===

# Exercise 3: Evaluate and Optimize RAG Performance

In this exercise, you will evaluate the performance of your RAG pipeline using Azure AI evaluators, implement various evaluation methods, and interpret the results to fine-tune your model. This ensures improved retrieval accuracy, response quality, and overall system efficiency.

## Objectives

In this exercise, you will complete the following tasks:

- Task 1: Evaluate with Azure AI evaluators
- Task 2: Implementing Evaluation Methods
- Task 3: Interpreting Results and Fine-Tuning 

### Task 1: Evaluate with Azure AI Evaluators

In this task, you will evaluate the RAG pipeline using Azure AI evaluators by analyzing key metrics such as coherence, relevance, and groundedness. You will modify the evaluation script to incorporate these metrics and log the results for further analysis.

1. Navigate back to **Visual Studio Code**. 

1. Expand the **assets (1)** folder and select **chat_eval_data.jsonl (2)**. This is an evaluation dataset, which contains example questions and expected answers (truth).

    ![](../media/af58.png)

1. Select the **evaluate.py** file.

    ![](../media/af59.png)

    - This script allows you to review the results locally by outputting them in the command line and putting them in a JSON file.
    - This script also logs the evaluation results to the cloud project so that you can compare evaluation runs in the UI.

1. To get *Coherence* and *Relevance* metrics along with *Groundedness*, add the following code to the **evaluate.py** file.    

1. Add the below import statement in the *<imports_and_config>* section, around the 10th or 11th line, before *# load environment variables from the .env file at the root of this repo*.

    ```bash
    from azure.ai.evaluation import CoherenceEvaluator, RelevanceEvaluator
    ```

    ![](../media/af60.png)    

1. Scroll down and add the below code before *# </imports_and_config>* and after *groundedness = GroundednessEvaluator(evaluator_model)*
.

    ```bash
    coherence = CoherenceEvaluator(evaluator_model)
    relevance = RelevanceEvaluator(evaluator_model)
    ```

    ![](../media/af61.png)    

1. Scroll down to the *<run_evaluation>* section, and around the 69th or 70th line, add the following code below *"groundedness": groundedness*.

    ```bash
    "coherence": coherence, 
    "relevance": relevance,
    ```

    ![](../media/af62.png)     

1. Press **Ctrl+S** to save the file.

### Task 2: Implementing Evaluation Methods      

In this task, you will implement evaluation methods to assess the performance of your RAG pipeline. You will install the necessary dependencies, run the evaluation script, and analyze metrics such as Groundedness, Coherence, and Relevance to ensure response quality.

1. From your console, run the below command to install the required package for running the evaluation script:

    ```bash
    pip install azure-ai-evaluation[remote]
    ```

    ![](../media/af65.png)

      >**Note:** Wait for the installation to complete. This might take some time.
      
      >**Note:** If you encounter any errors using this command, use the below command.

    ```bash
    pip install azure-ai-evaluation[remote] --use-deprecated=legacy-resolver
    ```

1. Please run the command below to install marshmallow.

    ```bash
    pip install --upgrade marshmallow==3.20.2
    ```

1. Now run the evaluation script:

    ```bash
    python evaluate.py
    ```

    ![](../media/af66.png)  
    
1. Once the upgrade is completed, rerun the command below.

    ```bash
    python evaluate.py
    ```

    ![](../media/af66.png) 

      >**Note**: Expect the evaluation to take around 5 - 10 minutes to complete.  

      >**Note**: You might see some time-out errors, which are expected. The evaluation script is designed to handle these errors and continue running.  

1. In the console output, you will see an answer for each question, followed by a table with summarized metrics. (You might see different columns in your output.)

    ```Text
    ====================================================
    '-----Summarized Metrics-----'
    {'groundedness.gpt_groundedness': 1.6666666666666667,
    'groundedness.groundedness': 1.6666666666666667}
    '-----Tabular Result-----'
                                        outputs.response  ... line_number
    0   Could you specify which tent you are referring...  ...           0
    1   Could you please specify which camping table y...  ...           1
    2   Sorry, I only can answer queries related to ou...  ...           2
    3   Could you please clarify which aspects of care...  ...           3
    4   Sorry, I only can answer queries related to ou...  ...           4
    5   The TrailMaster X4 Tent comes with an included...  ...           5
    6                                            (Failed)  ...           6
    7   The TrailBlaze Hiking Pants are crafted from h...  ...           7
    8   Sorry, I only can answer queries related to ou...  ...           8
    9   Sorry, I only can answer queries related to ou...  ...           9
    10  Sorry, I only can answer queries related to ou...  ...          10
    11  The PowerBurner Camping Stove is designed with...  ...          11
    12  Sorry, I only can answer queries related to ou...  ...          12

    [13 rows x 8 columns]
    ('View evaluation results in Azure AI Foundry portal: '
    'https://xxxxxxxxxxxxxxxxxxxxxxx')
    ```

    ![](../media/af67.png)   

      >**Note**: You might see some time-out errors, which are expected. The evaluation script is designed to handle these errors and continue running.   

### Task 3: Interpreting Results and Fine-Tuning         

In this task, you will interpret the evaluation results and fine-tune the RAG pipeline by adjusting the prompt template. You will analyze the **Relevance, Groundedness, and Coherence** scores, modify the prompt instructions, and rerun the evaluation to improve response accuracy.

1. Once the evaluation run is complete, **Ctrl+click** on the link to view the evaluation results on the Evaluation page in the Azure AI Foundry portal **(1)**, then click on **Open (2)**.

    ![](../media/af68.png)

1. On the **Report** tab, you can view the RAG App quality through the Metric dashboard.

1. You can view the average score for `Relevance, Groundedness`, and `Coherence`.

   ![image](../media/day3ex3-003.png)  

1. Navigate to the **Data (1)** tab for more details about the evaluation metric **(2)**.

    ![](../media/day3ex3-002.png)

1. Notice that the responses are not well grounded. The model often replies with a question rather than an answer. This is a result of the prompt template instructions.

1. In your **assets/grounded_chat.prompty (1)** file, find the sentence, `"If the question is not related to outdoor/camping gear and clothing, just say 'Sorry, I only can answer queries related to outdoor/camping gear and clothing. So, how can I help?"`. **(2)**

    ![](../media/af74.png)

1. Change the sentence to `If the question is related to outdoor/camping gear and clothing but vague, try to answer based on the reference documents, then ask for clarifying questions.`

    ![](../media/af75.png)

1. Press **Ctrl+S** to save the file.

1. Rerun the evaluation script.    

    ```bash
    python evaluate.py
    ```

     >**Note**: Expect the evaluation to take around 5 - 10 minutes to complete.  

     >**Note**: If you cannot increase the tokens per minute limit for your model, you might see some time-out errors, which are expected. The evaluation script is designed to handle these errors and continue running.

1. Once the evaluation run is complete, **Ctrl+click** on the link to view the evaluation results on the Evaluation page in the Azure AI Foundry portal **(1)**, then click on **Open (2)**.

    ![](../media/af68.png)    

1. On the **Report** tab, you can view the `Relevance, Groundedness`, and `Coherence` average scores, which have increased more than before.

    ![](../media/day3ex3-001.png)

1. Navigate to the **Data (1)** tab for more details about the evaluation metric **(2)**.

    ![](../media/day3ex3-004.png)    

1. Try other prompt template modifications to see how the changes affect the evaluation results.    

### Review

This exercise focused on evaluating and optimizing the performance of a Retrieval-Augmented Generation (RAG) system. Participants used Azure AI evaluators to assess retrieval accuracy, implemented evaluation methods to measure response quality and interpreted results to fine-tune the system for improved efficiency and relevance.

In this exercise, you have accomplished the following:
- Task 1: Evaluated with Azure AI evaluators
- Task 2: Implemented Evaluation Methods
- Task 3: Interpreted Results and Fine-Tuning 


### You have successfully finished the lab. 

===

# Exercise 4: Semantic Kernel Fundamentals

### Estimated Duration: 25 minutes

This hands-on lab provides practical experience with Semantic Kernel and the Azure AI Foundry GPT-4o model. Designed for those new to AI development, the lab guides you step-by-step to build an intelligent chat feature within a starter application. You will use the Semantic Kernel framework to connect with the GPT-4o model, implement a chat API that sends user prompts, and return dynamic AI-generated responses.

**Note:**- This lab is implemented in both **C#** and **Python**. You can perform the exercises in **whichever language you're comfortable with**—the core concepts remain the same. To view the instructions for a specific language:
- Click on the small **arrow icon** (▶) next to the language name.
- This will reveal the step-by-step instructions for that language.

Choose your preferred language and get started!

## Objectives
In this exercise, you will be performing the following tasks:
- Task 1: Setup environment variables
- Task 2: Update the code files and run the app

## Task 1: Setup environment variables

In this task, you will explore different flow types in Azure AI Foundry by setting up Visual Studio Code, retrieving Azure OpenAI credentials, and configuring them in Python and C# environments.

1. Open **Visual Studio code** from the desktop shortcut in the labvm.
1. Click on **File (1)** and select **Open Folder (2)**.

    ![](./media/image_023.png)
1. Navigate to `C:\LabFiles\Day-3-Custom-RAG-and-Semantic-Kernel` (1) and select **Semantic-Kernel (2)** folder and click on **Select Folder**.

1. If you receive `Do you trust the authors of the files in folder` warning, select the checkbox (1) and click on **Yes, I trust the authors (2)**.

    ![](./media/image_025.png)

1. Navigate to **Azure AI Foundry** Icon on top left.

1. Select the AI foundry project that you created earlier in the lab i.e. **ai-foundry-project-<inject key="Deployment ID" enableCopy="false"></inject> (1)**

1. On **Overview** page select **Azure OpenAI(1)** and **Copy (2)** the endpoint and paste it in **Notepad**, as it will be used in the upcoming exercises.

    ![](./media/focus8.png)
1. Copy the API key from AI Foundry Portal and paste it in **Notepad**, as it will be used in the upcoming exercises.

    ![](./media/focus1000.png)

<details>
<summary><strong>Python</strong></summary>

1. Navigate to `Python>src` directory and open **.env** (1) file.

    ![](./media/image_026.png)
1. Paste **Azure OpenAI Endpoint** copied earlier in the exercise besides `AZURE_OPENAI_ENDPOINT`.
    >Note:- Ensure that every value in the **.env** file is enclosed in **double quotes ("")**.
1. Paste **API key** copied earlier in the exercise besides `AZURE_OPENAI_API_KEY`.

    ![](./media/image_027.png)
	
1. Save the file.

</details>

<details>
<summary><strong>C Sharp(C#)</strong></summary>

1. Navigate to `Dotnet>src>BlazorAI` directory and open **appsettings.json (1)** file.

    ![](./media/image_028.png)
1. Paste **Azure OpenAI Endpoint** copied earlier in the exercise besides `AOI_ENDPOINT`.
    >**Note**:- Ensure that every value in the **appsettings.json** file is enclosed in **double quotes (")**.

    >**Note**:- Make sure to remove the "/" from the endpoint.
1. Paste **API key** copied earlier in the exercise besides `AOI_API_KEY`.

    ![](./media/image_029.png)
1. Save the file.

</details>

## Task 2: Update the code files and run the app

In this task, you will explore different flow types in Azure AI Foundry by updating code files, running the AI-powered app in Python and C#, and testing responses to user prompts.

<details>
<summary><strong>Python</strong></summary>

1. Navigate to `Python>src` directory and open **chat.py** file.

    ![](./media/image_030.png)
1. Add the following code in the `#Import Modules` (1) section of the file.
    ```
    from semantic_kernel.connectors.ai.chat_completion_client_base import ChatCompletionClientBase
    from semantic_kernel.connectors.ai.open_ai import OpenAIChatPromptExecutionSettings
    import os
    ```

    ![](./media/image_031.png)
1. Add the following code in the `# Challenge 02 - Chat Completion Service` (1) section of the file.
    ```
    chat_completion_service = AzureChatCompletion(
        deployment_name=os.getenv("AZURE_OPENAI_CHAT_DEPLOYMENT_NAME"),
        api_key=os.getenv("AZURE_OPENAI_API_KEY"),
        endpoint=os.getenv("AZURE_OPENAI_ENDPOINT"),
        service_id="chat-service",
    )
    kernel.add_service(chat_completion_service)
    execution_settings = kernel.get_prompt_execution_settings_from_service_id("chat-service")
    ```

    ![](./media/image_032.png)
1. Add the following code in the `# Start Challenge 02 - Sending a message to the chat completion service by invoking kernel` section of the file.
    ```
    global chat_history
    chat_history.add_user_message(user_input)
    chat_completion = kernel.get_service(type=ChatCompletionClientBase)
    execution_settings = kernel.get_prompt_execution_settings_from_service_id("chat-service")
    response = await chat_completion.get_chat_message_content(
        chat_history=chat_history,
        settings=execution_settings,
        kernel=kernel
    )
    chat_history.add_assistant_message(str(response))
    ```

    ![](./media/image_033.png)
1. Add the following code in the `#return result` section of the file.
    ```
    logger.info(f"Response: {response}")
    return response
    ```

    ![](./media/image_034.png)
1. In case you encounter any indentation error, use the code from the following URL:
    ```
    https://raw.githubusercontent.com/CloudLabsAI-Azure/ai-developer/refs/heads/prod/CodeBase/python/lab-02.py
    ```
1. Save the file.
1. Right click on `Python>src` in the left pane and select **Open in Integrated Terminal**.

    ![](./media/image_035.png)

1. Run the below command to install the packages from the requirements.txt file:

    ```
    pip install -r requirements.txt 
    ``` 

1. Run the below command to upgrade semantic kernel:

    ```
    pip install --upgrade semantic-kernel --pre
    ```   

1. Use the following command to run the app:
    ```
    streamlit run app.py
    ```
1. If you are asked for any email to register, feel free to use the below provided email, and hit **Enter**:
    ```
    test@gmail.com
    ```

    ![](./media/image_036.png)
1. If the app does not open automatically in the browser, you can access it using the following **URL**:

    ```
    http://localhost:8501
    ```
1. Submit the following prompt and see how the AI responds:
    ```
    Why is the sky blue?
    ```
    ```
    Why is it red?
    ```
1. You will receive a response similar to the one shown below:

    ![](./media/image_037.png)
</details>

<details>
<summary><strong>C Sharp(C#)</strong></summary>

1. Navigate to `Dotnet>src>BlazorAI>Components>Pages` directory and open **Chat.razor.cs (1)** file.

    ![](./media/image_038.png)
1. Add the following code in the `// Your code goes here` (Line no. 92) (1) section of the file.
    ```
    chatHistory.AddUserMessage(userMessage);
    var chatCompletionService = kernel.GetRequiredService<IChatCompletionService>();
    var assistantResponse = await chatCompletionService.GetChatMessageContentAsync(
        chatHistory: chatHistory,
        kernel: kernel);
    chatHistory.AddAssistantMessage(assistantResponse.Content);
    ```

    ![](./media/image_039.png)
1. In case you encounter any indentation error, use the code from the following URL: 
    ```
    https://raw.githubusercontent.com/CloudLabsAI-Azure/ai-developer/refs/heads/prod/CodeBase/c%23/lab-02.cs
    ```
1. Save the file.
1. Right click on `Dotnet>src>Aspire>Aspire.AppHost` in the left pane and select **Open in Integrated Terminal**.

    ![](./media/image_040.png)
1. Run the following line of code to trust the dev-certificates neccessary to run the app locally, and then select on **Yes**:
    ```
    dotnet dev-certs https --trust
    ```

    ![](./media/image_041.png)
1. Use the following command to run the app:
    ```
    dotnet run
    ```
1. Open a new tab in browser and navigate to the link for **blazor-aichat** i.e **https://localhost:7118/**.

    >**Note**: If you receive security warnings in the browser, close the browser and follow the link again.
1. Submit the following prompt and see how the AI responds:
    ```
    Why is the sky blue?
    ```
    ```
    Why is it red?
    ```
1. You will receive a response similar to the one shown below:

    ![](./media/image_042.png)
</details>

## Review

In this exercise, we utilized **Semantic Kernel** in combination with the **Azure AI Foundry GPT-4o model** to build an intelligent chat feature within a starter application. We integrated the Semantic Kernel framework with GPT-4o, implemented a chat API to handle user prompts, and returned dynamic AI-generated responses. This enhanced our proficiency in connecting applications to powerful language models using modern AI development frameworks.

Successfully completed the below tasks for AI-driven chat implementation using **Semantic Kernel** and **Azure AI Foundry GPT-4o**:  

- Integrated **Semantic Kernel** with **GPT-4o** for intelligent AI interactions.  
- Configured a **chat API** to process user prompts and generate AI-driven responses.     
- Extended chatbot functionality by integrating **Azure AI Search** for contextual data retrieval.  

## Select **Next** to proceed to the next lab.

===

# Exercise 5: Semantic Kernel Plugins

### Estimated Duration: 50 minutes

This hands-on lab explores the power of plugins in enhancing LLM development with Semantic Kernel. Designed for those new to AI extensibility, the lab guides you through building and integrating plugins to expand the capabilities of your chatbot. You will implement a time plugin and a weather retrieval plugin, enabling your AI to access real-time and contextual data beyond its training scope. Additionally, you will learn to develop Semantic Kernel plugins in Python and leverage Auto Function Calling to seamlessly chain them together.

**Note:**- This lab is implemented in both **C#** and **Python**. You can perform the exercises in **whichever language you're comfortable with**—the core concepts remain the same. To view the instructions for a specific language:
- Click on the small **arrow icon** (▶) next to the language name.
- This will reveal the step-by-step instructions for that language.

Choose your preferred language and get started!

## Objectives
In this exercise, you will be performing the following tasks:
- Task 1: Try the app without the Time Plugin
- Task 2: Create and import the Time Plugin
- Task 3: Create and import the Geocoding Plugin
- Task 4: Create and import the Weather Plugin

## Task 1: Try the app without the Time Plugin

In this task, you will explore different flow types in Azure AI Foundry by running the app without the Time Plugin to observe its default behavior.

1. Launch your AI Chat app in any of the programming language and submit the following prompt:
    ```
    What time is it?
    ```
2. Since the AI does not have the capability to provide real-time information, you will get a response similar to the following:

    ![](./media/image_043.png)

## Task 2: Create and import the Time Plugin

In this task, you will explore different flow types in Azure AI Foundry by creating and importing the Time Plugin to enhance the app's functionality.

<details>
<summary><strong>Python</strong></summary>

1. Navigate to `Python>src>plugins` directory and create a new file named **time_plugin.py (1)**.

    ![](./media/image_044.png)
1. Add the following code in the file: 

    ```
    from datetime import datetime
    from typing import Annotated
    from semantic_kernel.functions import kernel_function

    class TimePlugin:
        @kernel_function()
        def current_time(self) -> str:
            return datetime.now().strftime("%Y-%m-%d %H:%M:%S")

        @kernel_function()
        def get_year(self, date_str: Annotated[str, "The date string in format YYYY-MM-DD"] = None) -> str:
            if date_str is None:
                return str(datetime.now().year)
            
            try:
                date_obj = datetime.strptime(date_str, "%Y-%m-%d")
                return str(date_obj.year)
            except ValueError:
                return "Invalid date format. Please use YYYY-MM-DD."

        @kernel_function()
        def get_month(self, date_str: Annotated[str, "The date string in format YYYY-MM-DD"] = None) -> str:
            if date_str is None:
                return datetime.now().strftime("%B")
            
            try:
                date_obj = datetime.strptime(date_str, "%Y-%m-%d")
                return date_obj.strftime("%B")  # Full month name
            except ValueError:
                return "Invalid date format. Please use YYYY-MM-DD."

        @kernel_function()
        def get_day_of_week(self, date_str: Annotated[str, "The date string in format YYYY-MM-DD"] = None) -> str:
            if date_str is None:
                return datetime.now().strftime("%A")
            
            try:
                date_obj = datetime.strptime(date_str, "%Y-%m-%d")
                return date_obj.strftime("%A")  # Full weekday name
            except ValueError:
                return "Invalid date format. Please use YYYY-MM-DD."
    ```
1. Save the file.
1. Navigate to `Python>src` directory and open **chat.py** file.

    ![](./media/image_030.png)
1. Add the following code in the `#Import Modules` section of the file.
    ```
    from semantic_kernel.connectors.ai.open_ai.prompt_execution_settings.azure_chat_prompt_execution_settings import (
        AzureChatPromptExecutionSettings,
    )
    from plugins.time_plugin import TimePlugin
    ```
    
    ![](./media/image_045.png)
1. Add the following code in the `#Challenge 03 - Create Prompt Execution Settings` section of the file.
    ```
    execution_settings = AzureChatPromptExecutionSettings()
    execution_settings.function_choice_behavior = FunctionChoiceBehavior.Auto()
    logger.info("Automatic function calling enabled")
    ```

    ![](./media/image_046.png)
1. Add the following code in the `# Placeholder for Time plugin` section of the file.
    ```
    time_plugin = TimePlugin()
    kernel.add_plugin(time_plugin, plugin_name="TimePlugin")
    logger.info("Time plugin loaded")
    ```

    ![](./media/placeholder.png)

1. Search (using Ctrl+F) and remove the following piece of code from the file as we will enable automatic function calling and this is no longer required:
    ```
    execution_settings = kernel.get_prompt_execution_settings_from_service_id("chat-service")
    ```
    >**Note**: You need to remove it from two code blocks, one will be inside **def initialize_kernel():** function and another will be in **global chat_history** code block.
1. In case you encounter any indentation error, use the code from the following URL:
    ```
    https://raw.githubusercontent.com/CloudLabsAI-Azure/ai-developer/refs/heads/prod/CodeBase/python/lab-03_time_plugin.py
    ```
1. Save the file.
1. Right click on `Python>src` in the left pane and select **Open in Integrated Terminal**.

    ![](./media/image_035.png)
1. Use the following command to run the app:
    ```
    streamlit run app.py
    ```
1. If the app does not open automatically in the browser, you can access it using the following **URL**:
    ```
    http://localhost:8501
    ```
1. Submit the following prompt:
    ```
    What time is it?
    ```
1. Since the AI have the **Time Plugin**, it will be able to provide real-time information, you will get a response similar to the following:

    ![](./media/image_048.png)
</details>

<details>
<summary><strong>C Sharp(C#)</strong></summary>

1. Navigate to `Dotnet>src>BlazorAI>Plugins` directory and create a new file named **TimePlugin.cs**.

    ![](./media/image_049.png)
1. Add the following code in the file:
    ```
    using System;
    using System.ComponentModel;
    using System.Globalization;
    using Microsoft.SemanticKernel;

    namespace BlazorAI.Plugins
    {
        public class TimePlugin
        {        
            [KernelFunction("current_time")]
            [Description("Gets the current date and time from the server. Use this directly when the user asks what time it is or wants to know the current date.")]
            public string CurrentTime()
            {
                return DateTime.Now.ToString("yyyy-MM-dd HH:mm:ss");
            }

            [KernelFunction("get_current_time")]
            [Description("Gets the current date and time from the server's system clock. Use this directly without asking the user for their location.")]
            public string GetCurrentTime()
            {
                return DateTime.Now.ToString("yyyy-MM-dd HH:mm:ss");
            }
            
            [KernelFunction("get_year")]
            [Description("Extract the year from a date string or get the current year from the system clock. Examples: 'What year is it now?' or 'What year is 2023-05-15?'")]
            public string GetYear(
                [Description("The date string. Accepts formats like YYYY-MM-DD, MM/DD/YYYY, etc. If not provided, uses the server's current date.")] 
                string? dateStr = null)
            {
                if (string.IsNullOrEmpty(dateStr))
                {
                    return DateTime.Now.Year.ToString();
                }

                DateTime date;
                if (TryParseDate(dateStr, out date))
                {
                    return date.Year.ToString();
                }
                
                return $"Could not parse '{dateStr}' as a valid date. Please provide a date in a standard format like YYYY-MM-DD or MM/DD/YYYY.";
            }
            
            [KernelFunction("get_month")]
            [Description("Extract the month name from a date string or get the current month from the system clock. Examples: 'What month is it now?' or 'What month is 2023-05-15?'")]
            public string GetMonth(
                [Description("The date string. Accepts formats like YYYY-MM-DD, MM/DD/YYYY, etc. If not provided, uses the server's current date.")] 
                string? dateStr = null)
            {
                if (string.IsNullOrEmpty(dateStr))
                {
                    return DateTime.Now.ToString("MMMM");
                }
                
                DateTime date;
                if (TryParseDate(dateStr, out date))
                {
                    return date.ToString("MMMM"); // Full month name
                }
                
                return $"Could not parse '{dateStr}' as a valid date. Please provide a date in a standard format like YYYY-MM-DD or MM/DD/YYYY.";
            }
            
            [KernelFunction("get_day_of_week")]
            [Description("Get the day of week from the server's system clock or for a specific date. Examples: 'What day is it today?' or 'What day of the week is 2023-05-15?'")]
            public string GetDayOfWeek(
                [Description("The date string. Accepts formats like YYYY-MM-DD, MM/DD/YYYY, etc. If not provided, uses the server's current date.")] 
                string? dateStr = null)
            {
                if (string.IsNullOrEmpty(dateStr))
                {
                    return DateTime.Now.ToString("dddd");
                }
                
                DateTime date;
                if (TryParseDate(dateStr, out date))
                {
                    return date.ToString("dddd"); // Full day name
                }
                
                return $"Could not parse '{dateStr}' as a valid date. Please provide a date in a standard format like YYYY-MM-DD or MM/DD/YYYY.";
            }

            private bool TryParseDate(string dateStr, out DateTime result)
            {
                string[] formats = { 
                    "yyyy-MM-dd", "MM/dd/yyyy", "dd/MM/yyyy", 
                    "M/d/yyyy", "d/M/yyyy", "MMM d, yyyy", 
                    "MMMM d, yyyy", "yyyy/MM/dd", "dd-MMM-yyyy"
                };
                
                return DateTime.TryParseExact(
                    dateStr, 
                    formats, 
                    CultureInfo.InvariantCulture,
                    DateTimeStyles.None, 
                    out result) || DateTime.TryParse(dateStr, out result);
            }
        }
    }
    ```
1. Save the file.
1. Navigate to `Dotnet>src>BlazorAI>Components>Pages` directory and open **Chat.razor.cs** file.

    ![](./media/image_038.png)
1. Add the following code in the `// Import Models` section of the file.
    ```
    using Microsoft.SemanticKernel.Connectors.OpenAI;
    using BlazorAI.Plugins;
    using System;
    ```

    ![](./media/image_050.png)
1. Search **private Kernel? kernel;** (using Ctrl+F)  and add the following piece of code below it:
    ```
    private OpenAIPromptExecutionSettings? promptSettings;
    ```

    ![](./media/image_051.png)
1. Search **chatHistory = [];** (using Ctrl+F)  and replace the line with the following piece of code:
    ```
    chatHistory = new ChatHistory();
    ```

    ![](./media/focus9.png)
1. Add the following code in the `// Challenge 03 - Create OpenAIPromptExecutionSettings` (1) section of the file.
    ```
    promptSettings = new OpenAIPromptExecutionSettings
    {
        ToolCallBehavior = ToolCallBehavior.AutoInvokeKernelFunctions,
        Temperature = 0.7,
        TopP = 0.95,
        MaxTokens = 800
    };
    ```

    ![](./media/image_053.png)
1. Add the following code in the `// Challenge 03 - Add Time Plugin` section of the file.
    ```
    var timePlugin = new Plugins.TimePlugin();
    kernel.ImportPluginFromObject(timePlugin, "TimePlugin");
    ```

    ![](./media/image_054.png)
1. Search **var assistantResponse = await chatCompletionService.GetChatMessageContentAsync** (using Ctrl+F)  and add the following line of code between chatHistory and kernel:
    ```
    executionSettings: promptSettings,
    ```
    >**Note**: The final piece of code will be similar to the code below:
    ```
    var assistantResponse = await chatCompletionService.GetChatMessageContentAsync(
        chatHistory: chatHistory,
        executionSettings: promptSettings,
        kernel: kernel);
    ```
    
    ![](./media/image_055.png)
1. In case you encounter any indentation error, use the code from the following URL:
    ```
    https://raw.githubusercontent.com/CloudLabsAI-Azure/ai-developer/refs/heads/prod/CodeBase/c%23/lab-03_time_plugin.cs
    ```
1. Save the file.
1. Right click on `Dotnet>src>Aspire>Aspire.AppHost` in the left pane and select **Open in Integrated Terminal**.

    ![](./media/image_040.png)

     > **Note**: Close all the running powershell sessions in the Visual Studio Code terminal & all the tabs in the browser before running `dotnet run`.

1. Use the following command to run the app:
    ```
    dotnet run
    ```
1. Open a new tab in browser and navigate to the link for **blazor-aichat** i.e **https://localhost:7118/**.
1. Submit the following prompt:
    ```
    What time is it?
    ```
1. Since the AI have the **Time Plugin**, it will be able to provide real-time information, you will get a response similar to the following:

    ![](./media/image_056.png)
</details>

## Task 3: Create and import the Geocoding Plugin

In this task, you will explore different flow types in Azure AI Foundry by creating and importing the Geocoding Plugin to enable location-based functionality.

1. Open a new tab in the browser and navigate to +++https://geocode.maps.co/+++ portal and click on **Free API key** button on the top.

    ![](./media/image_057.png)
1. Enter your details and click on **Create Account (1)**.

    ![](./media/image_058.png)

     > **Note**: Use your personal/work E-mail to register.

1. You will receive an e-mail, click on the link in the e-mail to verify your e-mail.
1. You will receive your free **geocoding API key**, save it notepad for further use.

<details>
<summary><strong>Python</strong></summary>

1. Navigate to `Python>src` directory and open **.env** file.

    ![](./media/image_026.png)
1. Paste the geocoding API key you received just now via e-mail besides `GEOCODING_API_KEY`.

    ![](./media/image_059.png)
    >Note:- Ensure that every value in the **.env** file is enclosed in **double quotes (")**.
1. Save the file.
1. Navigate to `Python>src` directory and open **chat.py** file.

    ![](./media/image_030.png)
1. Add the following code in the `#Import Modules` section of the file.
    ```
    from plugins.geo_coding_plugin import GeoPlugin
    ```

    ![](./media/image_030.png)
1. Add the following code in the `# Placeholder for Time plugin` section, after the **time plugin** in the file.
    ```
    kernel.add_plugin(
        GeoPlugin(),
        plugin_name="GeoLocation",
    )
    logger.info("GeoLocation plugin loaded")
    ```

    ![](./media/image_061.png)
1. In case you encounter any indentation error, use the code from the following URL:
    ```
    https://raw.githubusercontent.com/CloudLabsAI-Azure/ai-developer/refs/heads/prod/CodeBase/python/lab-03_geo_coding.py
    ```
1. Save the file.
1. Right click on `Python>src` in the left pane and select **Open in Integrated Terminal**.

    ![](./media/image_035.png)
1. Use the following command to run the app:
    ```
    streamlit run app.py
    ```
1. If the app does not open automatically in the browser, you can access it using the following **URL**:
    ```
    http://localhost:8501
    ```
1. Submit the following prompt:
    ```
    What are the geo-coordinates for Tampa, FL
    ```
1. Since the AI have the **Geocoding Plugin**, it will be able to provide real-time information, you will get a response similar to the following:

    ![](./media/image_062.png)
</details>

<details>
<summary><strong>C Sharp(C#)</strong></summary>

1. Navigate to `Dotnet>src>BlazorAI` directory and open **appsettings.json** file.

    ![](./media/image_028.png)
1. Paste the geocoding API key you received just now via e-mail besides `GEOCODING_API_KEY`.

    ![](./media/image_063.png)
    >Note:- Ensure that every value in the **appsettings.json** file is enclosed in **double quotes (")**.
1. Save the file.
1. Navigate to `Dotnet>src>BlazorAI>Components>Pages` directory and open **Chat.razor.cs** file.

    ![](./media/image_038.png)
1. Add the following code in the `// Challenge 03 - Add Time Plugin` section, after the **time plugin** in the file.
    ```
    var geocodingPlugin = new GeocodingPlugin(
        kernel.Services.GetRequiredService<IHttpClientFactory>(), 
        Configuration);
    kernel.ImportPluginFromObject(geocodingPlugin, "GeocodingPlugin");
    ```

    ![](./media/image_064.png)
1. In case you encounter any indentation error, use the code from the following URL:
    ```
    https://raw.githubusercontent.com/CloudLabsAI-Azure/ai-developer/refs/heads/prod/CodeBase/c%23/lab-03_geo_coding.cs
    ```
1. Save the file.
1. Right click on `Dotnet>src>Aspire>Aspire.AppHost` in the left pane and select **Open in Integrated Terminal**.

    ![](./media/image_040.png)

     > **Note**: Close all the running powershell sessions in the Visual Studio Code terminal & all the tabs in the browser before running `dotnet run`.

1. Use the following command to run the app:
    ```
    dotnet run
    ```
1. Open a new tab in browser and navigate to the link for **blazor-aichat** i.e **https://localhost:7118/**
1. Submit the following prompt:
    ```
    What are the geo-coordinates for Tampa, FL
    ```
1. Since the AI have the **Geocoding Plugin**, it will be able to provide real-time information, you will get a response similar to the following:
    ```
    The geo-coordinates for Tampa, FL are:

    Latitude: 27.9477595
    Longitude: -82.458444 
    ```

    ![](./media/image_065.png)

</details>

## Task 4: Create and import the Weather Plugin

In this task, you will explore different flow types in Azure AI Foundry by creating and importing the Weather Plugin to integrate weather-related functionality.

<details>
<summary><strong>Python</strong></summary>

1. Navigate to `Python>src>plugins` directory and create a new file named **weather_plugin.py**.

    ![](./media/image_066.png)
1. Add the following code in the file:
    ```
    from typing import Annotated
    import requests
    from semantic_kernel.functions import kernel_function
    import json
    from datetime import datetime, timedelta

    class WeatherPlugin:
        @kernel_function(description="Get weather forecast for a location up to 16 days in the future")
        def get_forecast_weather(self, 
                                latitude: Annotated[float, "Latitude of the location"],
                                longitude: Annotated[float, "Longitude of the location"],
                                days: Annotated[int, "Number of days to forecast (up to 16)"] = 16):
            
            # Ensure days is within valid range (API supports up to 16 days)
            if days > 16:
                days = 16
            
            url = (f"https://api.open-meteo.com/v1/forecast"
                f"?latitude={latitude}&longitude={longitude}"
                f"&daily=temperature_2m_max,temperature_2m_min,precipitation_sum,precipitation_probability_max,weather_code"
                f"&amp;current=temperature_2m,relative_humidity_2m,apparent_temperature,precipitation,weather_code,wind_speed_10m"
                f"&temperature_unit=fahrenheit&wind_speed_unit=mph&precipitation_unit=inch"
                f"&forecast_days={days}&timezone=auto")
            
            try:
                response = requests.get(url)
                response.raise_for_status()
                data = response.json()
                
                daily = data.get('daily', {})
                times = daily.get('time', [])
                max_temps = daily.get('temperature_2m_max', [])
                min_temps = daily.get('temperature_2m_min', [])
                precip_sums = daily.get('precipitation_sum', [])
                precip_probs = daily.get('precipitation_probability_max', [])
                weather_codes = daily.get('weather_code', [])
                
                forecasts = []
                for i in range(len(times)):
                    # Convert date string to datetime object for day name
                    date_obj = datetime.strptime(times[i], "%Y-%m-%d")
                    day_name = date_obj.strftime("%A, %B %d")
                    
                    weather_desc = self._get_weather_description(weather_codes[i])
                    
                    forecast = {
                        "date": times[i],
                        "day": day_name,
                        "high_temp": f"{max_temps[i]}°F",
                        "low_temp": f"{min_temps[i]}°F",
                        "precipitation": f"{precip_sums[i]} inches",
                        "precipitation_probability": f"{precip_probs[i]}%",
                        "conditions": weather_desc
                    }
                    forecasts.append(forecast)
                
                result = {
                    "location_coords": f"{latitude}, {longitude}",
                    "forecast_days": len(forecasts),
                    "forecasts": forecasts
                }
                
                # For more concise output in chat
                return json.dumps(result, indent=2)
            except Exception as e:
                return f"Error fetching forecast weather: {str(e)}"
        
        def _get_weather_description(self, code):
            weather_codes = {
                0: "Clear sky",
                1: "Mainly clear", 2: "Partly cloudy", 3: "Overcast",
                45: "Fog", 48: "Depositing rime fog",
                51: "Light drizzle", 53: "Moderate drizzle", 55: "Dense drizzle",
                56: "Light freezing drizzle", 57: "Dense freezing drizzle",
                61: "Slight rain", 63: "Moderate rain", 65: "Heavy rain",
                66: "Light freezing rain", 67: "Heavy freezing rain",
                71: "Slight snow fall", 73: "Moderate snow fall", 75: "Heavy snow fall",
                77: "Snow grains",
                80: "Slight rain showers", 81: "Moderate rain showers", 82: "Violent rain showers",
                85: "Slight snow showers", 86: "Heavy snow showers",
                95: "Thunderstorm", 96: "Thunderstorm with slight hail", 99: "Thunderstorm with heavy hail"
            }
            return weather_codes.get(code, "Unknown")
    ```
1. Save the file.
1. Navigate to `Python>src` directory and open **chat.py** file.

    ![](./media/image_030.png)
1. Add the following code in the `#Import Modules` section of the file.
    ```
    from plugins.weather_plugin import WeatherPlugin
    ```

    ![](./media/image_067.png)
1. Add the following code in the `# Placeholder for Time plugin` section, after the **Geocoding plugin** in the file.
    ```
    kernel.add_plugin(
        WeatherPlugin(),
        plugin_name="Weather",
    )
    logger.info("Weather plugin loaded")
    ```

    ![](./media/image_068.png)
1. In case you encounter any indentation error, use the code from the following URL:
    ```
    https://raw.githubusercontent.com/CloudLabsAI-Azure/ai-developer/refs/heads/prod/CodeBase/python/lab-03_weather.py
    ```
1. Save the file.
1. Right click on `Python>src` in the left pane and select **Open in Integrated Terminal**.

    ![](./media/image_035.png)
1. Use the following command to run the app:
    ```
    streamlit run app.py
    ```
1. If the app does not open automatically in the browser, you can access it using the following **URL**:
    ```
    http://localhost:8501
    ```
1. Submit the following prompt:
    ```
    What is today's weather in San Francisco?
    ```
1. You will receive a response similar to the one shown below:

    ![](./media/image_069.png)

    The AI will perform the following plan to answer the question but may do so in a different order or different set of functions:

    1️⃣ The AI should ask Semantic Kernel to call the GetDate function on the Time Plugin to get today's date in order to calculate the number of days until next Thursday

    2️⃣ Because the Weather Forecast requires a Latitude and Longitude, the AI should instruct Semantic Kernel to call the GetLocation function on the Geocoding Plugin to get the coordinates for San Francisco

    3️⃣ Finally, the AI should ask Semantic Kernel to call the GetWeatherForecast function on the Weather Plugin passing in the current date/time and Lat/Long to get the weather forecast for Next Thursday (expressed as the number of days in the future) at the coordinates for San Francisco

    A simplified sequence diagram between Semantic Kernel and AI is shown below:

    ![](./media/seq_diag.png)

</details>
<details>
<summary><strong>C Sharp(C#)</strong></summary>

1. Navigate to `Dotnet>src>BlazorAI>Plugins` directory and create a new file named **WeatherPlugin.cs**.

    ![](./media/image_070.png)
1. Add the following code in the file:
    ```
    using System;
    using System.Collections.Generic;
    using System.ComponentModel;
    using System.Globalization;
    using System.Net.Http;
    using System.Text.Json;
    using System.Threading.Tasks;
    using Microsoft.SemanticKernel;

    namespace BlazorAI.Plugins
    {
        public class WeatherPlugin
        {
            private readonly IHttpClientFactory _httpClientFactory;

            public WeatherPlugin(IHttpClientFactory httpClientFactory)
            {
                _httpClientFactory = httpClientFactory;
            }

            [KernelFunction("GetWeatherForecast")]
            [Description("Get weather forecast for a location up to 16 days in the future")]
            public async Task<string> GetWeatherForecastAsync(
                [Description("Latitude of the location")] double latitude,
                [Description("Longitude of the location")] double longitude,
                [Description("Number of days to forecast (up to 16)")] int days = 16)
            {
                // Ensure days is within valid range (API supports up to 16 days)
                if (days > 16)
                    days = 16;

                var url = $"https://api.open-meteo.com/v1/forecast" +
                        $"?latitude={latitude}&longitude={longitude}" +
                        $"&daily=temperature_2m_max,temperature_2m_min,precipitation_sum,precipitation_probability_max,weather_code" +
                        $"&amp;current=temperature_2m,relative_humidity_2m,apparent_temperature,precipitation,weather_code,wind_speed_10m" +
                        $"&temperature_unit=fahrenheit&wind_speed_unit=mph&precipitation_unit=inch" +
                        $"&forecast_days={days}&timezone=auto";

                try
                {
                    var httpClient = _httpClientFactory.CreateClient();
                    var response = await httpClient.GetAsync(url);
                    response.EnsureSuccessStatusCode();
                    
                    var content = await response.Content.ReadAsStringAsync();
                    var data = JsonDocument.Parse(content);
                    
                    // Extract daily forecast data
                    var dailyElement = data.RootElement.GetProperty("daily");
                    var times = dailyElement.GetProperty("time").EnumerateArray().ToArray();
                    var maxTemps = dailyElement.GetProperty("temperature_2m_max").EnumerateArray().ToArray();
                    var minTemps = dailyElement.GetProperty("temperature_2m_min").EnumerateArray().ToArray();
                    var precipSums = dailyElement.GetProperty("precipitation_sum").EnumerateArray().ToArray();
                    var precipProbs = dailyElement.GetProperty("precipitation_probability_max").EnumerateArray().ToArray();
                    var weatherCodes = dailyElement.GetProperty("weather_code").EnumerateArray().ToArray();
                    
                    // Build a readable forecast for each day
                    var forecasts = new List<object>();
                    for (int i = 0; i < times.Length; i++)
                    {
                        // Convert date string to DateTime object for day name
                        var dateStr = times[i].GetString();
                        var dateObj = DateTime.Parse(dateStr!);
                        var dayName = dateObj.ToString("dddd, MMMM dd", CultureInfo.InvariantCulture);
                        
                        var weatherDesc = GetWeatherDescription(weatherCodes[i].GetInt32());
                        
                        var forecast = new
                        {
                            date = dateStr,
                            day = dayName,
                            high_temp = $"{maxTemps[i]}°F",
                            low_temp = $"{minTemps[i]}°F", 
                            precipitation = $"{precipSums[i]} inches",
                            precipitation_probability = $"{precipProbs[i]}%",
                            conditions = weatherDesc
                        };
                        
                        forecasts.Add(forecast);
                    }
                    
                    var result = new
                    {
                        location_coords = $"{latitude}, {longitude}",
                        forecast_days = forecasts.Count,
                        forecasts
                    };
                    
                    // For more concise output in chat
                    return JsonSerializer.Serialize(result, new JsonSerializerOptions { WriteIndented = true });
                }
                catch (Exception ex)
                {
                    return $"Error fetching forecast weather: {ex.Message}";
                }
            }
            
            [KernelFunction("GetForecastWithPlugins")]
            [Description("Gets weather forecast for any location by coordinating with Time and Geocoding plugins.")]
            public async Task<string> GetForecastWithPluginsAsync(
                [Description("The kernel instance to use for calling other plugins")] Kernel kernel,
                [Description("The location name (city, address, etc.)")] string location,
                [Description("The day of the week to get forecast for, or number of days in future")] string daySpec = "0")
            {
                try
                {
                    // Step 1: Get current date from Time Plugin
                    var dateResult = await kernel.InvokeAsync("Time", "GetDate");
                    string? todayStr = dateResult.GetValue<string>();
                    if (todayStr == null)
                    {
                        return "Could not determine the current date.";
                    }
                    DateTime today = DateTime.Parse(todayStr);
                    
                    // Step 2: Calculate target day based on specification
                    int daysInFuture;
                    if (int.TryParse(daySpec, out daysInFuture))
                    {
                        // If daySpec is a number, use it directly
                    }
                    else if (Enum.TryParse<DayOfWeek>(daySpec, true, out var targetDay))
                    {
                        // Calculate days until the next occurrence of the target day
                        daysInFuture = ((int)targetDay - (int)today.DayOfWeek + 7) % 7;
                        if (daysInFuture == 0) daysInFuture = 7; // If today is the target day, get next week
                    }
                    else
                    {
                        return $"Invalid day specification: {daySpec}. Please provide a day name or number of days.";
                    }
                    
                    // Step 3: Get location coordinates from Geocoding Plugin
                    var locationResult = await kernel.InvokeAsync("Geocoding", "GetLocation", new() { ["location"] = location });
                    string? locationJson = locationResult.GetValue<string>();
                    
                    if (locationJson == null)
                    {
                        return $"Could not get location data for: {location}";
                    }
                    
                    var locationData = JsonDocument.Parse(locationJson);
                    double latitude, longitude;
                    
                    try {
                        latitude = locationData.RootElement.GetProperty("latitude").GetDouble();
                        longitude = locationData.RootElement.GetProperty("longitude").GetDouble();
                    }
                    catch (Exception)
                    {
                        return $"Could not extract coordinates for location: {location}";
                    }
                    
                    // Step 4: Get weather forecast
                    return await GetWeatherForecastAsync(latitude, longitude, daysInFuture + 1);
                }
                catch (Exception ex)
                {
                    return $"Error coordinating weather forecast: {ex.Message}";
                }
            }

            private string GetWeatherDescription(int code)
            {
                var weatherCodes = new Dictionary<int, string>
                {
                    { 0, "Clear sky" },
                    { 1, "Mainly clear" }, { 2, "Partly cloudy" }, { 3, "Overcast" },
                    { 45, "Fog" }, { 48, "Depositing rime fog" },
                    { 51, "Light drizzle" }, { 53, "Moderate drizzle" }, { 55, "Dense drizzle" },
                    { 56, "Light freezing drizzle" }, { 57, "Dense freezing drizzle" },
                    { 61, "Slight rain" }, { 63, "Moderate rain" }, { 65, "Heavy rain" },
                    { 66, "Light freezing rain" }, { 67, "Heavy freezing rain" },
                    { 71, "Slight snow fall" }, { 73, "Moderate snow fall" }, { 75, "Heavy snow fall" },
                    { 77, "Snow grains" },
                    { 80, "Slight rain showers" }, { 81, "Moderate rain showers" }, { 82, "Violent rain showers" },
                    { 85, "Slight snow showers" }, { 86, "Heavy snow showers" },
                    { 95, "Thunderstorm" }, { 96, "Thunderstorm with slight hail" }, { 99, "Thunderstorm with heavy hail" }
                };
                
                return weatherCodes.TryGetValue(code, out var description) ? description : "Unknown";
            }
        }
    }
    ```
1. Save the file.
1. Navigate to `Dotnet>src>BlazorAI>Components>Pages` directory and open **Chat.razor.cs** file.

    ![](./media/image_038.png)
1. Add the following code in the `// Challenge 03 - Add Time Plugin` section, after the **geocoding plugin** in the file.
    ```
    var weatherPlugin = new WeatherPlugin(
        kernel.Services.GetRequiredService<IHttpClientFactory>());
        kernel.ImportPluginFromObject(weatherPlugin, "WeatherPlugin");
    ```

    ![](./media/image_071.png)
1. In case you encounter any indentation error, use the code from the following URL:
    ```
    https://raw.githubusercontent.com/CloudLabsAI-Azure/ai-developer/refs/heads/prod/CodeBase/c%23/lab-03_weather.cs
    ```
1. Save the file.
1. Right click on `Dotnet>src>Aspire>Aspire.AppHost` in the left pane and select **Open in Integrated Terminal**.

    ![](./media/image_040.png)
1. Use the following command to run the app:
    ```
    dotnet run
    ```
1. Open a new tab in browser and navigate to the link for **blazor-aichat** i.e **https://localhost:7118/**.
1. Submit the following prompt:
    ```
    What is today's weather in San Francisco?
    ```

    The AI will perform the following plan to answer the question but may do so in a different order or different set of functions:

    1️⃣ The AI should ask Semantic Kernel to call the GetDate function on the Time Plugin to get today's date in order to calculate the number of days until next Thursday

    2️⃣ Because the Weather Forecast requires a Latitude and Longitude, the AI should instruct Semantic Kernel to call the GetLocation function on the Geocoding Plugin to get the coordinates for San Francisco

    3️⃣ Finally, the AI should ask Semantic Kernel to call the GetWeatherForecast function on the Weather Plugin passing in the current date/time and Lat/Long to get the weather forecast for Next Thursday (expressed as the number of days in the future) at the coordinates for San Francisco

    A simplified sequence diagram between Semantic Kernel and AI is shown below:

    ![](./media/seq_diag.png)

</details>

## Review

In this exercise, we utilized **Semantic Kernel plugins** to enhance LLM capabilities by extending a chatbot's functionality. We developed and integrated a **time plugin** and a **weather retrieval plugin** to enable real-time, contextual responses beyond the model’s training data. Additionally, we built plugins in Python and used **Auto Function Calling** to chain them together seamlessly. This enhanced our proficiency in building extensible, intelligent AI solutions using Semantic Kernel.

Successfully completed the below tasks for extending **LLM capabilities** using **Semantic Kernel plugins**:  

- Developed and integrated a **time plugin** and a **weather retrieval plugin** for real-time contextual responses.  
- Utilized **Semantic Kernel** to enhance chatbot functionality beyond the model’s training data.  
- Implemented **Auto Function Calling** to seamlessly chain multiple plugins together.  
- Built and deployed **Python-based plugins** to extend AI capabilities.  

## Congratulations.  You have completed this lab.
