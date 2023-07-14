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
##### Create the following services:
- Watson Assistant
- Watson Discovery

Create the service instances
- If you do not have an IBM Cloud account, register for a free trial account here.
- Create a Assistant instance from the catalog.
- Create a Discovery instance from the catalog and select the default "Plus" plan.

```
NOTE: The first instance of the Plus plan for IBM Watson Discovery comes with a free 30-day trial; it is chargeable once the trial is over. If you no longer require your Plus instance for Watson Discovery after going through this exercise, feel free to delete it.
```

### 3. Configure Watson Discovery
Start by launching your Watson Discovery instance.
From the IBM Cloud dashboard, click on your new Discovery service in the resource list.
![image](https://github.com/mikhaeladitha/business-assistant/assets/81720515/1c74f381-31d6-4f42-b33e-e2cbaee122b1)
From the `Manage` tab panel of your Discovery service, click the `Launch Watson Discovery` button.

#### Create a project and collection
The landing page for Watson Discovery is a panel showing your current projects.

Create a new project by clicking the New Project tile.
```
NOTE: The Watson Discovery service queries are defaulted to be performed on all collections within a project. For this reason, it is advised that you create a new project to contain the collection we will be creating for this code pattern.
```
![image](https://github.com/mikhaeladitha/business-assistant/assets/81720515/5ecb73cf-4318-4caa-b8dc-6bb8756cd9a0)

Give the project a unique name and select the Document Retrieval option, then click Next.

![image](https://github.com/mikhaeladitha/business-assistant/assets/81720515/772492ec-cc35-4e55-9f6a-bdaf6dd3364f)

For data source, click on the `Upload data` tile and click `Next`.

![image](https://github.com/mikhaeladitha/business-assistant/assets/81720515/9caaa7e4-2956-4562-bfe1-2929bec4ab01)

Enter a unique name for your collection and click `Finish`.

#### Import the document
On the `Configure Collection` panel, click the `Drag and drop files here or upload` button to select and upload the file located in the data directory of your local repo.

![image](https://github.com/mikhaeladitha/business-assistant/assets/81720515/86db0cc4-c950-470a-bcb2-ebb47ed9fa44)

Once the file is loaded, click the `Finish` button.

Note that after the file is loaded it may take some time for Watson Discovery to process the file and make it available for use. You should see a notification once the file is ready.

#### Access the collection
To access the collection, make sure you are in the correct project, then click the `Manage Collections` tab in the left-side of the panel.

![image](https://github.com/mikhaeladitha/business-assistant/assets/81720515/907551d6-18da-473d-bb0d-a5949f612d23)

Click the collection tile to access it.

![image](https://github.com/mikhaeladitha/business-assistant/assets/81720515/81cff7b4-8577-4db2-838c-d60b76a11eb3)

Before applying Smart Document Understanding (SDU) to our document, lets do some simple queries on the data so that we can compare it to results found after applying SDU. Click the `Try it out` panel to bring up the query panel.

![image](https://github.com/mikhaeladitha/business-assistant/assets/81720515/4d9f6954-2258-48da-922b-2d157e33fce8)

Enter queries related to the operation of the thermostat and view the results. As you will see, the results are not very useful, and in some cases, not even related to the question.

#### Annotate with SDU
Now let's apply SDU to our document to see if we can generate some better query responses.

![image](https://github.com/mikhaeladitha/business-assistant/assets/81720515/221cf36f-bfca-47fb-9017-cd3739a49e85)

From the  `Define structure` drop-down menu, click on `New fields`.

![image](https://github.com/mikhaeladitha/business-assistant/assets/81720515/18adc4d4-9220-4197-992a-27f422732351)

From the `Identify fields` tab panel, click on `User-trained models`, the click `Submit` from the confirmation dialog.

Click the `Apply changes and reprocess` button in the top right corner. This will SDU process.

Here is the layout of the SDU annotation panel:

![image](https://github.com/mikhaeladitha/business-assistant/assets/81720515/ee60a052-6851-4e90-ab16-3affd18a2f3f)

The goal is to annotate all of the pages in the document so Discovery can learn what text is important, and what text can be ignored.
- [1] is the list of pages in the manual. As each is processed, a green check mark will appear on the page.
- [2] is the current page being annotated.
- [3] is a graphic display of the same page, but allows you to select regions that you can label.
- [4] is the list of labels you can assign to the graphic page.
- Click [5] to submit the page to Discovery.
- Click [6] when you have completed the annotation process.

To add/change a label, select the new label, then click on the text areas in the graphic page to apply it.

As you go though the annotations one page at a time, Discovery is learning and should start automatically updating the upcoming pages. Once you get to a page that is already correctly annotated, you can stop, or simply click Submit [5] to acknowledge it is correct. The more pages you annotate, the better the model will be trained.

For this specific owner's manual, at a minimum, it is suggested to mark the following:

- The main title page as title
- The table of contents (shown in the first few pages) as table_of_contents
- All headers and sub-headers (typed in light green text) as a subtitle
- All page numbers as footers
- All circuitry diagram pages (located near the end of the document) as a footer
- All licensing information (located in the last few pages) as a footer
- All other text should be marked as text.

Click the Apply changes and reprocess button [6] to load your changes. Wait for Discovery to notify you that the reprocessing is complete.

Next, click on the Manage fields [1] tab.

![image](https://github.com/mikhaeladitha/business-assistant/assets/81720515/8a4eeb79-4c25-4bd4-b6d1-4ca58aed243e)

- [2] Here is where you tell Discovery which fields to ignore. Using the on/off buttons, turn off all labels except subtitles and text.
- [3] is telling Discovery to split the document apart, based on subtitle.
- Click [4] to submit your changes.

Return to the Activity tab. After the changes are processed (may take some time), your collection will look very different:

![image](https://github.com/mikhaeladitha/business-assistant/assets/81720515/fd923528-a08b-4fd2-bd7e-a301efd6b47c)

Return to the query panel (click Try it out) and see how much better the results are.

![image](https://github.com/mikhaeladitha/business-assistant/assets/81720515/8856b8e2-fb0c-46aa-8118-68e139dbfbfa)

### 4. Configure Watson Assistant
Find the Assistant service in your IBM Cloud Dashboard.

Click on the service and then click on Launch tool.

#### Create assistant
From the main Assistant panel, you will see 2 tab options - Assistants and Skills. An Assistant is the container for a set of Skills.

Go to the Assistant tab and click Create assistant.

![image](https://github.com/mikhaeladitha/business-assistant/assets/81720515/70eec75f-04bc-4e32-87ca-2762ff04bc56)

Give your assistant a unique name then click Create assistant.

#### Create Assistant dialog skill
You will then be taken to a panel that shows the Skills assigned to your Assistant. You can also revisit this panel by selected the desired Assistant listed in the Assistants tab panel.

![image](https://github.com/mikhaeladitha/business-assistant/assets/81720515/9668d5ae-9737-466d-8114-4c745fdb5ce2)

Click on `Add dialog skill`.

From the Add Dialog Skill panel, select the `Use sample skill` tab.

Select the `Customer Care Sample Skill` as your template.

The newly created dialog skill should now be shown in your Assistant panel:

![image](https://github.com/mikhaeladitha/business-assistant/assets/81720515/5f70aa86-256b-4efe-8601-6db173d9ab9f)

Click on your newly created dialog skill to edit it.

#### Add new intent
The default customer care dialog does not have a way to deal with any questions involving outside resources, so we will need to add this.

Create a new intent that can detect when the user is asking about operating the Ecobee thermostat.

From the Customer Care Sample Skill panel, select the Intents tab.

Click the Create intent button.

Name the intent #Product_Information, and at a minimum, enter the following example questions to be associated with it.

![image](https://github.com/mikhaeladitha/business-assistant/assets/81720515/8e1dfdee-642a-4f3f-81e9-4d3940742d11)

#### Create new dialog node
Now we need to add a node to handle our intent. Click on the back arrow in the top left corner of the panel to return to the main panel. Click on the `Dialog` tab to bring up the nodes defined for the dialog.

Select the `What can I do` node, then click on the drop down menu and select the Add node below option.

![image](https://github.com/mikhaeladitha/business-assistant/assets/81720515/6f71bded-0ac6-4873-b6f7-b58359ab50d6)

Name the node "Ask about product" [1] and assign it our new intent [2].

![image](https://github.com/mikhaeladitha/business-assistant/assets/81720515/e3ef5e4b-5d29-4ec7-bb9d-ba00acf63507)

In the Assistant responds dropdown, select the option Search skill.

This means that if Watson Assistant recognizes a user input such as "how do I set the time?", it will direct the conversation to this node, which will integrate with the search skill.

#### Create Assistant search skill
Return to the Skills panel by clicking the Skills icon in the left menu pane.

From your Assistant skills panel, click on Create skill.

For Skill Type, select Search skill and click Next.
```
Note: If you have provisioned Watson Assistant on IBM Cloud, the search skill is only offered on a paid plan, but a 30-day trial version is available if you click on the Plus button.
```
Give your search skill a unique name, then click Continue.

From the search skill panel, select the Discovery service instance and collection you created previously.

