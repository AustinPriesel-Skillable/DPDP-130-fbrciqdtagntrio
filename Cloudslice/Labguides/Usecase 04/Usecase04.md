## Usecase 04 - Connect Fabric Data Agent to Microsoft Foundry for unified and intelligent data insights
**Introduction**

Modern organizations generate large volumes of data across multiple
systems, making it difficult for business users and analysts to access
insights quickly. Data is often stored in silos, requiring technical
expertise to extract, analyze, and interpret information.

The Microsoft Fabric unified data platform addresses this challenge by
bringing together analytics, data engineering, and business intelligence
capabilities into a single environment. By integrating agent-based AI
capabilities from Azure AI Foundry, organizations can build intelligent
applications that interact with enterprise data using natural language
and automated workflows.

The Agentic Applications for Unified Data Foundation Solution
Accelerator demonstrates how AI-powered agents can leverage unified
enterprise data to answer questions, automate data analysis, and deliver
insights to both technical and non-technical users. These agentic
applications orchestrate tasks, retrieve relevant data, and generate
contextual responses, enabling faster decision-making and improved
operational efficiency.

In this use case, organizations can use AI agents to analyze business
data such as sales performance, customer behavior, and product trends.
Instead of manually querying multiple datasets, users can simply ask
questions in natural language and receive actionable insights directly
from the system.

**Objective**

The objective of this use case is to demonstrate how organizations can
leverage **agentic AI with a unified data foundation** to improve data
accessibility and decision-making. The key goals include:

**Establish a Unified Data Foundation in Microsoft Fabric**

- Create a governed Fabric workspace with Lakehouse, Warehouse, and
  semantic models.

- Load and validate enterprise datasets for analysis.

**2. Build and Configure a Fabric Data Agent**

- Create a **Fabric Data Agent** capable of querying datasets using
  natural language.

- Connect Ontology resources and define agent instructions to support
  enterprise-specific queries.

**3. Deploy Azure and Foundry Components**

- Provision Azure resources including Foundry project, AI services,
  search, storage, and app services.

- Deploy supporting components through Azure Developer CLI (azd).

**4. Connect the Fabric Data Agent to Microsoft Foundry**

- Create or configure an AI agent within Foundry.

- Link the agent to Microsoft Fabric using Workspace ID and AI Skills
  ID.

- Provide domain-specific instructions enabling the agent to interpret
  and analyze Fabric data.

**5. Enable Conversational Analytics and Automated Insights**

- Test the agent in Foundry Playground with real business queries.

- Demonstrate natural language-to-data retrieval workflows using Fabric
  Lakehouse datasets.

- Deliver insights such as inspection pass/fail rates, averages, trends,
  and grouped summaries.

**6. Showcase End-to-End Agentic Application Workflow**

- Integrate Foundry agent, Fabric data source, and Azure infrastructure
  into a functional web application.

- Validate intelligent data interactions, automated reasoning, and
  insight generation.

**Solution architecture**

![Architecture Diagram](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image1.png)

The solution combines Microsoft Fabric and Microsoft Foundry to create
an AI solution that can answer questions using both structured data and
unstructured documents:

- **Microsoft Fabric** provides the data layer with Lakehouse,
  Warehouse, and the Fabric IQ semantic layer for natural language to
  SQL translation

- **Microsoft Foundry** hosts the AI agents, including Foundry IQ for
  document retrieval and the Orchestrator Agent that orchestrates both
  capabilities

- **Azure AI Services** powers the language models (GPT-4o-mini) and
  embeddings

- **Azure AI Search** stores document vectors for semantic retrieval

## Prerequisites

- **GitHub Account: You are expected to have your own GitHub login
  credentials.  
  If you do not have an account, please create one by visiting:  
  +++https://github.com/signup?user_email=&source=form-home-signup+++**

## Task 0: Create a GitHub account

In this task, you create a new **Github account** with the same tenant
credentials that you have used in this lab.

1.  Navigate to the GitHub with this link
    +++https://github.com/+++ and click on **Sign up** to proceed
    further.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image2.png)

2.  Now, to create a new GitHub account, enter
    the **email**, **password** and a unique **username** and click
    on **Continue** button.

    ![A screenshot of a login box AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image3.png)

3.  Start the **verification** **puzzle** by following the instruction
    on the screen. Click on **Submit.**

4.  Enter the **verification** **code** you've received on your mail.

    ![A screenshot of a email form AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image4.png)

5.  Now, with your credentials sign-in to GitHub and click on **Sign
    in.**

    ![A screenshot of a login page AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image5.png)

6.  You have successfully created a new account on GitHub.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image6.png)

## Task 1: Create a Fabric workspace

In this task, you create a Fabric workspace. The workspace contains all
the items needed for this lakehouse tutorial, which includes lakehouse,
dataflows, Data Factory pipelines, the notebooks, Power BI datasets, and
reports.

1.  Open your browser, navigate to the address bar, and type or paste
    the following
    URL: +++https://app.fabric.microsoft.com/+++ press the **Enter** button
    and sign in with your credentials

    |   |   |
    |---|---|
    | Username | +++@lab.CloudPortalCredential(User1).Username+++ |
    | TAP | +++@lab.CloudPortalCredential(User1).AccessToken+++ |

2.  Fabric home page, select **+New workspace** tile.

    ![A screenshot of a computer Description automatically
generated](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image7.png)

3.  In the **Create a workspace** pane that appears on the right side,
    enter the following details, and click on the **Apply** button.

    | Property | Value |
    |---------|-------|
    | Name | +++Fabric agent@lab.LabInstance.Id+++  |
    | Advanced | Under **License mode**, select **Fabric** |
    | Default storage format | Small dataset storage format |
    | Template apps | Check **Develop template apps** |

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image8.png)

    >[!Note] To find your lab instant ID, select 'Help' and copy the instant
    ID.

    ![A screenshot of a computer Description automatically
generated](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image9.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image10.png)

4.  Wait for the deployment to complete. It takes 2-3 minutes to
    complete.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image11.png)

## Task 2: Retrieve your Fabric workspace ID

You will need your workspace ID to pass as a parameter when building the
solution.

1.  Look at the URL - the workspace ID is the GUID that appears
    after /groups/:

2.  Copy the **Workspace ID** from the URL (e.g.,
    https://app.fabric.microsoft.com/groups/{workspace-id}/...) and save
    it in **Notepad** for later use

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image12.png)

## Task 3: Open development environment

1.  Open your browser, navigate to the address bar, type or paste the
    following URL: +++https://github.com/technofocus-pte/agnticapp-for-unified-data/tree/main+++

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image13.png)

2.  Click on **fork** to fork the repo. Give unique name to the repo and
    click on **Create repo** button.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image14.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image15.png)

3.  Click on **Code -\> Codespaces -\> Create Codespace on main**

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image16.png)

4.  Wait for the Codespaces environment to setup. It takes few minutes
    to setup completely

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image17.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image18.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image19.png)

## Task 4: Provision services and deploy the application to Azure and Fabric

1.  Run the following command on the Terminal. It generates the code to
    copy. Copy the code and press Enter.

    +++azd auth login+++

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image20.png)

2.  Default browser opens to enter the generated code to verify. Enter
    the code and click **Next**.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image21.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image22.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image23.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image24.png)

3.  Login to Azure:

    +++az login+++

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image25.png)

4.  Default browser opens to enter the generated code to verify. Enter
    the code and click **Next**.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image26.png)

5.  Select your azure subscription

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image27.png)

6.  Register the Microsoft Cognitive Services resource provider
    (required if not already registered on your subscription)

    +++az provider register --namespace Microsoft.CognitiveServices+++

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image28.png)

	>[!alert] Navigate to the **Infra** folder on the left hand side, open the **main.bicep** file on **line 122** and switch the *Lab Instance ID* string to +++@lab.LabInstance.Id+++.
	
8.  Provision and deploy all the resources:

    +++azd up+++

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image29.png)

9.  Select below values.

    - **To create an environment for Azure resources**,
      enter +++env@lab.LabInstance.Id+++
    
    - **Select an Azure Subscription to
      use** : **@lab.CloudSubscription.Name**
    
    - **'Location' infrastructure
      parameter:** **Select the ResourceGroup1 Location**
    
    - **Resource Group:** **@lab.CloudResourceGroup(ResourceGroup1).Name**
  
## Note: If the Codespace deployment fails in the selected Azure region, update the deployment region and rerun the deployment.
```bash
azd env set AZURE_RESOURCE_LOCATION <region>
```
For example:
```bash
azd env set AZURE_RESOURCE_LOCATION westus2
```
Supported regions:
* `westus2`
* `japaneast`
* `swedencentral`
* `northeurope`
After updating the region, rerun the deployment steps.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image30.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image31.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image32.png)

10.  This deployment will take *7-10 minutes* to provision the resources
    in your account and set up the solution with sample data.

11. Now the deployment is complete

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image33.png)

12. Create and activate a virtual environment

    +++python -m venv .venv+++

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image34.png)

13. Use the top-left **menu icon** in **Visual Studio Code**, then
    navigate to **Terminal → New Terminal** to open a new terminal
    window in the workspace

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image35.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image36.png)

14. Run the following command in the terminal to install the required
    Python dependencies

    +++pip install uv && uv pip install -r scripts/requirements.txt+++

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image37.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image38.png)

15. Run the following command on the Terminal. It generates the code to
    copy. Copy the code and press Enter.

    +++az login+++

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image39.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image40.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image41.png)

16. Select your **Azure subscription** from the list to continue the
    setup process.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image42.png)

17. Run the bash script from the output of the azd deployment. Replace
    the with your Fabric workspace Id created in the previous steps. The
    script will look like the following:

    ```
    python scripts/00_build_solution.py --from 02 --fabric-workspace-id <your-workspace-id>
    ```


    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image43.png)

18. Press Enter to start create resources

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image44.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image45.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image46.png)

## Task 5: Review the Fabric Lakehouse and Data

1.  Go to your +++https://app.fabric.microsoft.com/+++ workspace

2.  Make sure resource got deployed successfully

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image47.png)

3.  Click on the **Lakehouse** to verify that the data has been
    successfully loaded.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image48.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image49.png)

4.  Return to the **Codespace** to test the agent.

## Task 6: Test the agent

1.  To test the agent, run the following command in the terminal.

    +++python scripts/08_test_agent.py+++

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image50.png)

2.  Enter the sample questions +++What is the average score from inspections?+++

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image51.png)

    +++What constitutes a failed inspection?+++

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image52.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image53.png)

	+++Do any inspections violate quality control standards in our Inspection Procedures?+++

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image54.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image55.png)

3.  Press **Ctrl+C** to cancel the process.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image56.png)

## Task 7: Create a Fabric data agent

1.  Go to your +++https://app.fabric.microsoft.com/+++ Microsoft Fabric
    workspace

2.  Select "New item" → Search for "Data Agent" → select Data
    agent

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/img1.png)

4.  Provide the name +++FabricDataAgent@lab.LabInstance.Id+++ and click
    **Create**

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image58.png)

4.  Select **Add data source** to configure a new data source.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image59.png)

6.  Select your Ontology resource for this workshop

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image60.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image61.png)

6.  Click **Agent instructions** from top menu.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image62.png)

7.  Add the below agent instructions:

     +++You are a helpful assistant that can answer user questions using data. Support group by in GQL+++

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image63.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image64.png)

8.  Click Publish from the top menu and select Publish.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image65.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image66.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image67.png)

    >[!Note] The Ontology set up may take up to 15 minutes so retry after some time if you don't see good responses.

5.  To test the agent, run the application and enter the sample
    questions to verify the responses.

    +++How many tickets are high priority+++

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image68.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image69.png)

    +++What is the average score from inspections?+++

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image70.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image71.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image72.png)
    
    +++Show tickets grouped by status+++

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image73.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image74.png)

6.  Save the **Workspace ID** and **AISkills ID** in **Notepad** for
    later use

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image75.png)

7.  Return to the **Codespace** to deploy and launch the application.

## Task 8: Deploy and launch the application

1.  Run the following command to set the **AZURE_ENV_DEPLOY_APP**
    environment variable to **true** before deployment.

    +++azd env set AZURE_ENV_DEPLOY_APP true+++

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image76.png)

2.  Run azd up - This will provision Azure resources

    +++azd up+++

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image77.png)

3.  Once the deployment has completed successfully, copy the Web app URL

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image78.png)

4.  Run the following command to set up app permissions

    +++python scripts/00_build_solution.py --from 09+++

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image79.png)

5.  Press Enter to start configuration

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image80.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image81.png)

6.  Click on the app URL

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image82.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image83.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image84.png)

    > **Sample Questions**
    >
    > To help you get started, here are some **Sample Questions** you can
    > ask in the app:
    >
    > For Retail sales analysis use case:

     +++Show total revenue by year for last 5 years+++.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image85.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image86.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image87.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image88.png)

    > [!Alert] You may not see a response due to the populated date, please continue with the rest of the lab.

## Task 9: Verify Azure Resources and Review Fabric Lakehouse Data 

1.  Open a browser go to +++https://portal.azure.com+++ and sign in with
    your cloud slice account below.

2.  Select **Resource groups**

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image89.png)

3.  Click on your assigned **Resource group**.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image90.png)

4.  Make sure the below resource got deployed successfully

    - Foundry
    
    - Foundry project
    
    - Application Insights
    
    - Search service
    
    - Azure Storage account
    
    - App Service
    
    - Azure Cosmos DB account

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image91.png)

## Task 10: Consume Fabric data agent from Microsoft Foundry Services

1.  Select **Foundry**

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image92.png)

2.  On the Overview pane, click on **Go to Foundry portal**. This will
    navigate you to the Microsoft Foundry portal.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image93.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image94.png)

    Once navigated to Foundry Portal, select **Agents** from the left menu
    you will already see an agent **pre created**. If not created, then
    please click on the **+ New agent** option to get it created.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image95.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image96.png)

3.  Select the newly created **agent**, and a configuration pane will be
    opened on the right. Enter the agent name as +++Fabric Agent+++

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image97.png)

4.  In the same agent configuration pane, scroll down and click on **+
    Add** for **Knowledge** parameter.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image98.png)

5.  In the **Add knowledge** pane, select **Microsoft Fabric** 

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image99.png)

6.  Click on **+ Create connection**

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image100.png)

7.  Enter the custom keys, such as the **Workspace ID** and **AISkills
    ID**, that you saved in **Task 7\> Step 6**.Provide the connection
    name as **Fabric-aiskills**, and click **Connect.**

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image101.png)

8.  Enter Instructions

    ```
    You are a data assistant that analyzes inspection data stored in Microsoft Fabric.
    
    Use the Fabric Lakehouse dataset to answer questions about inspection results and scores. The dataset includes the following columns:
    - inspection_id: Unique identifier for each inspection
    - ticket_id: Identifier associated with the inspection ticket
    - result: Inspection outcome (Pass or Fail)
    - score: Numeric score assigned to the inspection
    
    You can analyze and summarize the data to provide insights such as:
    - Total number of inspections
    - Number of passed and failed inspections
    - Average, highest, and lowest inspection scores
    - Distribution of inspection results
    - Score trends across inspections or tickets
    
    When responding:
    - Use the Fabric data source to retrieve accurate information.
    - Provide clear summaries and insights based on the inspection results.
    - When appropriate, suggest visualizations such as bar charts or pie charts to show pass vs fail distribution or score comparisons.
    - Ensure answers are concise, accurate, and based only on the available dataset.
    ```

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image102.png)

9.  Select, **Agents** from left menu, and then choose the **Fabric
    Agent** agent and click on **Try in playground**.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image103.png)

10. A chat panel will open where you can enter your prompts. The agent
    will now respond using the documents and datasets you've connected.

    Sample prompts -
        
    +++What constitutes a failed inspection?+++

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image104.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image105.png)

    +++What is the total number of tickets in the system?+++

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image106.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image107.png)

    +++Do any inspections violate quality control standards in our
    Inspection Procedures?+++

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image108.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image109.png)

## Task 11: Delete the Resources

1.  To delete resources , type **Resource groups** in the Azure portal
    search bar, navigate and click on **Resource
    groups** under **Services**.

    ![A screenshot of a computer Description automatically
generated](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image110.png)

2.  In the Resource groups page, select your resource group.

3.  On the **Resource Group** home page, select all resources except
    **Fabric Capacity**, and then click **Delete**.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image111.png)

4.  In the **Delete Resources** pane that appears on the right side,
    navigate to **Enter "delete" to confirm deletion** field, then click
    on the **Delete** button

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image112.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image113.png)

9.  Go to your +++https://app.fabric.microsoft.com/+++ Microsoft Fabric
    workspace

10. Select the **...** option under the workspace name and
    select **Workspace settings**.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image114.png)

11. Select **General** and **Remove this workspace.**

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image115.png)

12. Click on **Delete** in the warning that pops up.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image116.png)

13. Wait for a notification that the Workspace has been deleted, before
    proceeding to the next lab.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2004/media/image117.png)

> **Summary**
>
> This use case demonstrates how organizations can build **intelligent,
> agent-driven data applications** by integrating **Microsoft Fabric**
> with **Microsoft Foundry**. The solution establishes a **unified data
> foundation** where enterprise data stored in Fabric Lakehouse and
> Warehouse can be accessed and analyzed through AI-powered agents.
>
> By connecting a **Fabric Data Agent** to Foundry, users can interact
> with enterprise datasets using **natural language queries** instead of
> writing complex SQL or manually analyzing multiple data sources. The
> AI agent retrieves relevant data, performs analysis, and generates
insights such as averages, trends, summaries, and grouped results.
>
> The solution also provisions supporting Azure services, including AI
> services, search, storage, and web applications, enabling a complete
> **end-to-end agentic application architecture**. This allows
> organizations to combine **structured enterprise data with AI
> capabilities** to deliver conversational analytics and automated
insights.
>
> Overall, the use case highlights how **agentic AI applications** built
> on a unified data platform can simplify data access, accelerate
> analytics, and support faster, data-driven decision-making for both
> technical and non-technical users.
