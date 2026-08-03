## Usecase 05 - Integrate Fabric Data Agent with Microsoft Teams for actionable insights and agent-to-agent collaboration using Copilot Studio

**Introduction**

In today’s competitive digital marketplace, e-commerce companies
generate large volumes of data from customer transactions, product
catalogs, website interactions, and payment systems. Extracting
meaningful insights from this data is essential for improving customer
experience, optimizing operations, and increasing revenue. However,
managing and analyzing large datasets from multiple sources can be
complex without a unified analytics platform.

**Zava**, a rapidly growing e-commerce company, sells various consumer
products through its online platform and processes thousands of orders
every day. The company collects data from different systems including
order management, customer profiles, product inventory, and payment
transactions. As Zava’s business grows, the organization faces
challenges in analyzing this data efficiently and providing real-time
insights to business teams.

To address these challenges, Zava implements a modern analytics solution
using **Microsoft Fabric**. Fabric provides a unified platform that
integrates data engineering, data storage, data transformation, and
business intelligence capabilities within a single environment. Zava
stores its raw and processed e-commerce data in the Fabric Lakehouse,
enabling scalable data management and analytics.

In addition, Zava leverages **Microsoft Fabric Data Agents** to enhance
data accessibility and insights generation. Fabric Agents allow business
users and analysts to interact with enterprise data using natural
language queries. Instead of manually searching through reports or
writing complex queries, users can simply ask questions such as:

- “What are the top selling products this month?”

- “Which region generated the highest sales revenue?”

- “What is the customer order trend over the last quarter?”

The Fabric Agent automatically retrieves relevant data from the
Lakehouse and generates insights, helping teams quickly understand
business performance. This intelligent interaction improves productivity
and enables faster decision-making across departments.

With this solution, Zava’s business users, analysts, and management
teams can easily explore data, monitor key performance indicators, and
gain real-time insights into sales performance, customer behavior, and
product demand. By combining advanced analytics with AI-powered Fabric
Agents, Zava builds a scalable and intelligent e-commerce analytics
platform that supports data-driven growth and operational excellence.

**Objectives**

- Build and configure a **Fabric Data Agent** connected to an e‑commerce
  semantic model.

- Ingest and model data inside a **Fabric Lakehouse** and expose it
  through a semantic model.

- Enhance the agent’s intelligence using **meta‑prompts** and
  agent‑level instructions.

- Connect the Fabric Data Agent to **Copilot Studio** and enable
  multi‑agent communication.

- Publish the Copilot agent and integrate it into **Microsoft Teams**
  for real‑time analytics.

- Test the end‑to‑end flow by querying business insights directly in
  Teams.

# Exercise 1: Creating and Configuring the Fabric Data Agent

## In this exercise, you will establish the foundational components in Microsoft Fabric. You will create a workspace, set up a lakehouse, ingest sample CSV datasets, generate a semantic model, and configure a Fabric Data Agent capable of answering analytical questions. This provides the core data intelligence layer used throughout the rest of the lab.

## Task 1: Create a Fabric workspace

In this task, you create a Fabric workspace. The workspace contains all
the items needed for this lakehouse tutorial, which includes lakehouse,
dataflows, Data Factory pipelines, the notebooks, Power BI datasets, and
reports.

1.  Open your browser, navigate to the address bar, and type or paste
    the following
    URL:+++https://app.fabric.microsoft.com/+++ press the **Enter** button
    and sign in with your credentials

    |   |   |
    |---|---|
    | Username | +++@lab.CloudPortalCredential(User1).Username+++ |
    | TAP | +++@lab.CloudPortalCredential(User1).AccessToken+++ |

2.  Fabric home page, select **+New workspace** tile.

    ![A screenshot of a computer Description automatically
generated](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2005/media/image1.png)

3.  In the **Create a workspace** pane that appears on the right side,
    enter the following details, and click on the **Apply** button.

    | Property | Value |
    |---------|-------|
    | Name | +++Fabric-Copilot-@lab.labinstance.id+++  |
    | Advanced | Under **License mode**, select **Fabric** |
    | Default storage format | Small dataset storage format |
    | Template apps | Check **Develop template apps** |

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2005/media/image2.png)

    >[!Note] To find your lab instant ID, select 'Help' and copy the instant
    ID.

    ![A screenshot of a computer Description automatically generated](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2005/media/image3.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2005/media/image4.png)

4.  Wait for the deployment to complete. It takes 2-3 minutes to
    complete.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2005/media/image5.png)

## Task 2: Create a lakehouse and Ingest sample data

In this task, you set up a lakehouse and ingest the NYC Taxi sample data
along with additional CSV files. This establishes your raw dataset
foundation inside Fabric, enabling you to start transformations and
queries later.

1.  Create a new lakehouse by clicking on the **+New item** button in
    the navigation bar.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2005/media/image6.png)

2.  On the **Filter by item type** search box,
    enter +++Lakehouse+++ and select the lakehouse item.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2005/media/image7.png)

3.  On the **New lakehouse** dialog box,
    enter +++fabricagent_lakehouse+++ in the **Name** field, click
    on the **Create** button and open the new lakehouse.

    >[!Note] Ensure to remove space before **fabricagent_lakehouse**.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2005/media/image8.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2005/media/image9.png)

4.  Wait for the notification stating **Successfully created SQL
    endpoint**.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2005/media/image10.png)

5.  In the **lakehouse** page, navigate to **Get data in your
    lakehouse** section, and click on **Upload files**

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2005/media/image11.png)

6.  On the Upload files tab, click on the folder under the Files

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2005/media/image12.png)

7.  Browse to **C:\LabFiles** on your VM, then select **customers.csv,
    Orders_Data.csv** and **products.csv** file and click
    on **Open** button.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2005/media/image13.png)

8.  Then, click on the **Upload** button and close

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2005/media/image14.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2005/media/image15.png)

9.  Click and select refresh on the **Files**. The file appears.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2005/media/image16.png)

10. In the **Lakehouse** page, Under the Explorer pane select Files.
    Now, however your mouse **Orders_Data.csv** file. Click on the
    horizontal ellipses **(…)** beside **Orders_Data.csv** . Navigate
    and click on **Load Table**, then select **New table**.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2005/media/image17.png)

11. In the **Load file to new table** dialog box, click on
    the **Load** button.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2005/media/image18.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2005/media/image19.png)

12. Repeat the same process for **customers.csv** and **products.csv**
    to convert them into tables.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2005/media/image20.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2005/media/image21.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2005/media/image22.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2005/media/image23.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2005/media/image24.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2005/media/image25.png)

13. Select **SQL analytics endpoint** from the **Lakehouse** drop-down
    menu at the top right of the screen.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2005/media/image26.png)

14. From the lakehouse **Home** tab, select **New semantic model** and
    select the tables that you want to add to the semantic model.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2005/media/image27.png)

16. In the **New semantic model** dialog enter +++E-commerce Order
    Dataset+++ and then select the **all** tables from the list of
    tables and select **Confirm** to create the new model.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2005/media/image28.png)

15. From the left menu, select **Fabric-Copilot-@lab.labinstance.id** workspace icon
    and then select workspace name.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2005/media/image29.png)

## Task 3: Create a Fabric data agent

1.  In the **Fabric-Copilot-@lab.labinstance.id** workspace page, navigate and click
    on **+New item** button, then select **Data agent**

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2005/media/image30.png)

2.  Provide the name +++DataAgent_@lab.LabInstance.Id+++ and click
    **Create**

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2005/media/image31.png)

3.  Select **Add data source** to configure a new data source.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2005/media/image32.png)

4.  Select **E-commerce Order Dataset ** (Type: Semantic Model) from the
    results.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2005/media/image33.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2005/media/image34.png)

5.  When you first ask the questions with the listed tables select **all
    tables** the data agent answers them fairly well.

6.  For instance, for the question +++Who are the top 10 customers by total purchase amount?+++

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2005/media/image35.png)

7.  Run the application and enter the sample questions to verify the
    responses.

    +++Which day has the highest sales?+++

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2005/media/image36.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2005/media/image37.png)

## Task 4: Optimizing with Meta-Prompts

1.  In the **Setup** section, locate the **Agent instructions** field.
    (Alternatively, you can also locate the **Agent instructions** in
    the navigation bar on the top.)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2005/media/image38.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2005/media/image39.png)

2.  Generate agent-level instructions using this meta-prompt in
    the **test pane** (on the right where it says **Test the agent's
    responses**):
    
    ```
    Meta-Prompt: Generate Agent-Level Instructions:
     Analyze your available data sources and create agent-level instructions for yourself (max 15000 chars).
    
     Objective: {AGENT_OBJECTIVE}
     Users: {USER_PERSONA}
    
     Examine your data sources: list all sources, types, and primary use. Analyze domain, time coverage, and main themes.
    
     Generate instructions with:
     ## Objective
     ## Data Sources (list with priority)
     ## Key Terminology (infer from columns/measures)
     ## Response Guidelines
     Style: {RESPONSE_STYLE}
     ## Handling Common Topics (3-5 based on available data)
    
     Custom terms: {CUSTOM_TERMINOLOGY}
    ```
    
    When using this meta-prompt, replace the variables manually in the
    prompt as per the values below **OR** paste these in the Test:

    - {AGENT_OBJECTIVE}: "E-commerce analytics agent for business
      intelligence"
    
    - {USER_PERSONA}: "Business analysts and sales teams"
    
    - {RESPONSE_STYLE}: "Clear summaries with data citations and trend
      analysis"
    
    - {CUSTOM_TERMINOLOGY}: Leave empty or add your domain-specific terms

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2005/media/image40.png)

8.  Test the enhanced agent with more complex queries:
    
     +++How many orders are placed each day?+++

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2005/media/image41.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2005/media/image42.png)

     +++Which products have the lowest stock levels?+++

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2005/media/image43.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2005/media/image44.png)

## Task 5: Publishing the Agent

1.  Generate the agent description using this meta-prompt inside the
    test pane of your Fabric Agent

    ```
     Meta-Prompt: Generate Agent Description
     Create a 1-2 sentence description of yourself as a Fabric Data Agent (max 200 chars).
    
     Analyze your data sources and describe: what data domain you cover and what questions you answer.
    
     Example: "Fabric Data Agent for retail sales. Answers questions about revenue, products, customers, and orders"
    
     Output plain text only.
    ```

     ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2005/media/image45.png)

3.  Click **Publish** and paste the generated description in the purpose
    and capabilities field.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2005/media/image46.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2005/media/image47.png)

# Exercise 2: Connecting Fabric Agent to Copilot Studio

This exercise focuses on enabling Copilot Studio to communicate with the Fabric Data Agent. You will create a Copilot agent, configure its behavior, link it to the Fabric Agent, and ensure that both agents collaborate to produce richer insights. This establishes multi‑agent communication across platforms.

## Task 1:Creating the Copilot Studio Agent

1.  Open a new browser tab and navigate to  +++https://copilotstudio.microsoft.com/+++.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2005/media/image48.png)

2.  In the left navigation, select **Agents**

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2005/media/image49.png)

3.  Click the blue **+Create blank agent** button.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2005/media/image50.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2005/media/image51.png)

4.  Click **Edit** to modify the settings.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2005/media/image52.png)

5.  Configure your agent with these settings:

    - **Name**: +++E-commerce RAG Agent+++

    - **Description**: +++An agent connected to a Microsoft Fabric data
      agent specializing in e-commerce business knowledge and support+++

    - Select your agent’s model, select **Claude Sonnet 4.5**

    - **Instructions**: Copy the instructions from the code block below

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2005/media/image53.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2005/media/image54.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2005/media/image55.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2005/media/image56.png)

6.  Click **Publish** in the top-right corner.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2005/media/image57.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2005/media/image58.png)

## Task 2: Adding the Fabric Agent as a connected agent for Copilot Studio

1.  After agent creation, navigate to the **Agents** tab and click
    **+Add agent**

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2005/media/image59.png)

2.  Click **Connect to an external agent** and select **Microsft Fabric (preview)**.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2005/media/image60.png)

3.  If it say's *Connection : Not connected* then click the drop down
    next to *Not connected* and select **Create new connection**. Verify
    it's showing as the email for your account, then click **Next**.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2005/media/image61.png)

4.  Click **Create** and sign in using the same account used for this lab

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2005/media/image62.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2005/media/image63.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2005/media/image64.png)

5.  Select your Fabric Data Agent

    - Look for the agent name you created in Exercise 1 Task 3

    - Click to select it

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2005/media/image65.png)

6.  Enter the **agent name** as +++DataAgent-@lab.LabInstance.Id+++,
    verify the **connection**, and then click **Add and configure** to
    proceed with the agent setup.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2005/media/image66.png)

7.  Click **Publish** to make the agent available for use

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2005/media/image67.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2005/media/image68.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2005/media/image69.png)

## Task 3: Testing the connected Fabric Data Agent

1.  Test the Fabric Data Agent connection with progressive queries:

    +++What are the top 10 highest value orders?+++

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2005/media/image70.png)

2.  Click **Allow** to grant the required permissions

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2005/media/image71.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2005/media/image72.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2005/media/image73.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2005/media/image74.png)

    >[!Note] The response generation process may take **5–6 minutes** to
    complete.

    +++What is the average price per category?+++

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2005/media/image75.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2005/media/image76.png)

    +++What percentage of orders use credit card vs PayPal vs debit card?+++

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2005/media/image77.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2005/media/image78.png)

    +++What is the revenue by payment method?+++

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2005/media/image79.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2005/media/image80.png)

# Exercise 3: Connect the Fabric Data Agent to Teams

In this exercise, you will publish the Copilot agent to Teams, enabling business users to access enterprise data directly from within their collaboration app. You will validate the agent’s functionality by running several BI queries and observing real‑time responses inside Teams.

## Task 1: Add Copilot Capabilities

1.  In the **E-commerce RAG Agent**, click the **+ (Add)** icon and
    select **Channels** to configure the agent channel settings.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2005/media/image81.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2005/media/image82.png)

2.  Select **Teams and Microsoft 365 Copilot**

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2005/media/image83.png)

3.  Click Add Channel

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2005/media/image84.png)

4.  Select **See agent in Teams** to open and test the agent in
    **Microsoft Teams**.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2005/media/image85.png)

5.  Click **Open Microsoft Teams**

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2005/media/image86.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2005/media/image87.png)

6.  Click **Sing in**

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2005/media/image88.png)

7.  Enter your provided credentials to sign in and continue

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2005/media/image89.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2005/media/image90.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2005/media/image91.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2005/media/image92.png)

8.  Click **Add**

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2005/media/image93.png)

9.  After the app is added successfully, click the *Open* button to
    launch the E‑commerce RAG Agent in Microsoft Teams

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2005/media/image94.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2005/media/image95.png)

## Task 2: Testing the connected Fabric Data Agent

1.  Test the Fabric Data Agent connection with progressive queries:

    +++What is the revenue trend over time?+++
   
    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2005/media/image96.png)

2.  Click **Allow** to grant the required permissions

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2005/media/image97.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2005/media/image98.png)

    +++What are the top 10 highest value orders?+++

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2005/media/image99.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2005/media/image100.png)

    +++Which payment method is used the most?+++

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2005/media/image101.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrciqdtagntsfrntr/refs/heads/main/Cloudslice/Labguide/Usecase%2005/media/image102.png)

**Summary**

This use case focuses on how an e‑commerce organization can integrate
**Microsoft Fabric Data Agents** with **Microsoft Teams** through
**Copilot Studio** to deliver real‑time insights, natural‑language
analytics, and multi‑agent collaboration. By combining a unified
analytics platform (Microsoft Fabric) with conversational AI (Copilot
Studio and Teams), business users can seamlessly access sales trends,
product insights, and customer behavior without writing queries. The
solution demonstrates how AI agents can retrieve data from Fabric
Lakehouse, enrich responses using instructions, and work alongside other
agents to streamline business intelligence workflows.

