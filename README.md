# Web-Based Virtual Assistant for Business Development
Using the Watson Discovery Smart Document Understanding (SDU) feature, we will enhance the Discovery model so that queries will be better focused to only search the most relevant information found in a typical owner's manual.

Using Watson Assistant, we will use a standard customer care dialog to handle a typical conversation between a customer and a company representitive. When a customer question involves operation of a product, the Assistant dialog skill will communicate with the Discovery service using an Assistant search skill.

## What is SDU?
SDU trains Watson Discovery to extract custom fields in your documents. Customizing how your documents are indexed into Discovery will improve the answers returned from queries.

With SDU, you annotate fields within your documents to train custom conversion models. As you annotate, Watson is learning and will start predicting annotations. SDU models can also be exported and used on other collections.

Current document type support for SDU is based on your plan:
- Lite plans: PDF, Word, PowerPoint, Excel, JSON, HTML
- Advanced plans: PDF, Word, PowerPoint, Excel, PNG, TIFF, JPG, JSON, HTML

## What is an Assistant Search Skill?
An Assistant search skill is a mechanism that allows you to directly query a Watson Discovery collection from your Assistant dialog. A search skill is triggered when the dialog reaches a node that has a search skill enabled. The user query is then passed to the Watson Discovery collection via the search skill, and the results are returned to the dialog for display to the user.

Click [here](https://cloud.ibm.com/docs/services/assistant?topic=assistant-skill-search-add) for more information about the Watson Assistant search skill.

## Flow
![image](https://github.com/mikhaeladitha/business-assistant/assets/81720515/e33af9a1-22e0-4ee6-a8d5-e263022e2af6)
1. The document is annotated using Watson Discovery SDU
2. The user interacts with the backend server via the app UI. The frontend app UI is a chatbot that engages the user in a conversation.
3. Dialog between the user and backend server is coordinated using a Watson Assistant dialog skill.
4. If the user asks a product operation question, a search query is issued to the Watson Discovery service via a Watson Assistant search skill.

## Steps
1. Clone the Repo
2. Create Watson services
3. Configure Watson Discovery
4. Configure Watson Assistant
5. Deploy Chatbot into web
6. Run the application

### 1. Clone the Repo
`git clone https://github.com/IBM/watson-assistant-with-search-skill`

### 2. Create Watson Discovery
Create the following services:
- Watson Assistant
- Watson Discovery

Create the service instances
- If you do not have an IBM Cloud account, register for a free trial account here.
- Create a Assistant instance from the catalog.
- Create a Discovery instance from the catalog and select the default "Plus" plan.

```
NOTE: The first instance of the Plus plan for IBM Watson Discovery comes with a free 30-day trial; it is chargeable once the trial is over. If you no longer require your Plus instance for Watson Discovery after going through this exercise, feel free to delete it.
```
