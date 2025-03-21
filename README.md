Disclaimer Click to show or hide
Tip: As you follow the instructions in this pane, whenever you see a icon, you can use it to copy text from the instruction pane into the virtual machine interface. This is particularly useful to copy code; but bear in mind you may need to modify the pasted code to fix indent levels or formatting before running it!

Log into Windows as Student account with the password Pa55w.rd.

During the lab exercise, use the following credentials to sign into the Azure subscription that is provided for you:

User name: User1-49443944@LODSPRODMCA.onmicrosoft.com
Password: J!2d!aK9Bn
Create all Azure resources in the ResourceGroup1 resource group.

Azure resources such as Virtual Machine names, sizes and locations must match what is specified in these steps. Due to the nature of Cloud Slice, deployments are limited to the scope defined in the instructional steps. If the resources provisioned differ from what is outlined in the instructions, a failure notification will be received, advising this was “disallowed by policy.” If you believe you have received this notification in error, please submit a support request for our team to investigate further.

Create a Question Answering Solution
One of the most common conversational scenarios is providing support through a knowledge base of frequently asked questions (FAQs). Many organizations publish FAQs as documents or web pages, which works well for a small set of question and answer pairs, but large documents can be difficult and time-consuming to search.

Azure AI Language includes a question answering capability that enables you to create a knowledge base of question and answer pairs that can be queried using natural language input, and is most commonly used as a resource that a bot can use to look up answers to questions submitted by users.

Provision an Azure AI Language resource
If you don't already have one in your subscription, you'll need to provision an Azure AI Language service resource. Additionally, to create and host a knowledge base for question answering, you need to enable the Question Answering feature.

Open the Azure portal at https://portal.azure.com, and sign in using the Microsoft account associated with your Azure subscription.

Select Create a resource.

In the search field, search for Language service. Then, in the results, select Create under Language Service.

Select the Custom question answering block. Then select Continue to create your resource. You will need to enter the following settings:

Subscription: Your Azure subscription
Resource group: Choose or create a resource group.
Region: Choose any available location
Name: Enter a unique name
Pricing tier: Select F0 (free), or S (standard) if F is not available.
Azure Search region: Choose a location in the same global region as your Language resource
Azure Search pricing tier: Free (F) (If this tier is not available, select Basic (B))
Responsible AI Notice: Agree
Select Create + review, then select Create.

NOTE Custom Question Answering uses Azure Search to index and query the knowledge base of questions and answers.

Wait for deployment to complete, and then go to the deployed resource.

View the Keys and Endpoint page. You will need the information on this page later in the exercise.

Create a question answering project
To create a knowledge base for question answering in your Azure AI Language resource, you can use the Language Studio portal to create a question answering project. In this case, you'll create a knowledge base containing questions and answers about Microsoft Learn.

In a new browser tab, go to the Language Studio portal at https://language.cognitive.azure.com/ and sign in using the Microsoft account associated with your Azure subscription.

If you're prompted to choose a Language resource, select the following settings:

Azure Directory: The Azure directory containing your subscription.
Azure subscription: Your Azure subscription.
Resource type: Language
Resource name: The Azure AI Language resource you created previously.
If you are not prompted to choose a language resource, it may be because you have multiple Language resources in your subscription; in which case:

On the bar at the top if the page, select the Settings (⚙) button.
On the Settings page, view the Resources tab.
Select the language resource you just created, and click Switch resource.
At the top of the page, click Language Studio to return to the Language Studio home page.
At the top of the portal, in the Create new menu, select Custom question answering.

In the *Create a project wizard, on the Choose language setting page, select the option to Select the language for all projects, and select English as the language. Then select Next.

On the Enter basic information page, enter the following details:

Name LearnFAQ
Description: FAQ for Microsoft Learn
Default answer when no answer is returned: Sorry, I don't understand the question
Select Next.

On the Review and finish page, select Create project.

Add sources to the knowledge base
You can create a knowledge base from scratch, but it's common to start by importing questions and answers from an existing FAQ page or document. In this case, you'll import data from an existing FAQ web page for Microsoft Learn, and you'll also import some pre-defined "chit chat" questions and answers to support common conversational exchanges.

On the Manage sources page for your question answering project, in the ╋ Add source list, select URLs. Then in the Add URLs dialog box, select ╋ Add url and set the following name and URL before you select Add all to add it to the knowledge base:
Name: Learn FAQ Page
URL: https://docs.microsoft.com/en-us/learn/support/faq
On the Manage sources page for your question answering project, in the ╋ Add source list, select Chitchat. The in the Add chit chat dialog box, select Friendly and select Add chit chat.
Edit the knowledge base
Your knowledge base has been populated with question and answer pairs from the Microsoft Learn FAQ, supplemented with a set of conversational chit-chat question and answer pairs. You can extend the knowledge base by adding additional question and answer pairs.

In your LearnFAQ project in Language Studio, select the Edit knowledge base page to see the existing question and answer pairs (if some tips are displayed, read them and choose Got it to dismiss them, or select Skip all)

In the knowledge base, on the Question answer pairs tab, select ＋, and create a new question answer pair with the following settings:

Source: https://docs.microsoft.com/en-us/learn/support/faq
Question: What are Microsoft credentials?
Answer: Microsoft credentials enable you to validate and prove your skills with Microsoft technologies.
Select Done.

In the page for the What are Microsoft credentials? question that is created, expand Alternate questions. Then add the alternate question How can I demonstrate my Microsoft technology skills?.

In some cases, it makes sense to enable the user to follow up on an answer by creating a multi-turn conversation that enables the user to iteratively refine the question to get to the answer they need.

Under the answer you entered for the certification question, expand Follow-up prompts and add the following follow-up prompt:

Text displayed in the prompt to the user: Learn more about credentials.
Select the Create link to new pair tab, and enter this text: You can learn more about credentials on the [Microsoft credentials page](https://docs.microsoft.com/learn/credentials/).
Select Show in contextual flow only. This option ensures that the answer is only ever returned in the context of a follow-up question from the original certification question.
Select Add prompt.

Train and test the knowledge base
Now that you have a knowledge base, you can test it in Language Studio.

Save the changes to your knowledge base by selecting the Save button under the Question answer pairs tab on the left.
After the changes have been saved, select the Test button to open the test pane.
In the test pane, at the top, deselect Include short answer response (if not already unselected). Then at the bottom enter the message Hello. A suitable response should be returned.
In the test pane, at the bottom enter the message What is Microsoft Learn?. An appropriate response from the FAQ should be returned.
Enter the message Thanks! An appropriate chit-chat response should be returned.
Enter the message Tell me about Microsoft credentials. The answer you created should be returned along with a follow-up prompt link.
Select the Learn more about credentials follow-up link. The follow-up answer with a link to the certification page should be returned.
When you're done testing the knowledge base, close the test pane.
Deploy the knowledge base
The knowledge base provides a back-end service that client applications can use to answer questions. Now you are ready to publish your knowledge base and access its REST interface from a client.

In the LearnFAQ project in Language Studio, select the Deploy knowledge base page from the navigation menu on the left.
At the top of the page, select Deploy. Then select Deploy to confirm you want to deploy the knowledge base.
When deployment is complete, select Get prediction URL to view the REST endpoint for your knowledge base and note that the sample request includes parameters for:
projectName: The name of your project (which should be LearnFAQ)
deploymentName: The name of your deployment (which should be production)
Close the prediction URL dialog box.
Prepare to develop an app in Visual Studio Code
You'll develop your question answering app using Visual Studio Code. The code files for your app have been provided in a GitHub repo.

Tip: If you have already cloned the mslearn-ai-language repo, open it in Visual Studio code. Otherwise, follow these steps to clone it to your development environment.

Start Visual Studio Code.

Open the palette (SHIFT+CTRL+P) and run a Git: Clone command to clone the https://github.com/MicrosoftLearning/mslearn-ai-language repository to a local folder (it doesn't matter which folder).

When the repository has been cloned, open the folder in Visual Studio Code.

Note: If Visual Studio Code shows you a pop-up message to prompt you to trust the code you are opening, click on Yes, I trust the authors option in the pop-up.

Wait while additional files are installed to support the C# code projects in the repo.

Note: If you are prompted to add required assets to build and debug, select Not Now.

Configure your application
Applications for both C# and Python have been provided, as well as a sample text file you'll use to test the summarization. Both apps feature the same functionality. First, you'll complete some key parts of the application to enable it to use your Azure AI Language resource.

In Visual Studio Code, in the Explorer pane, browse to the Labfiles/02-qna folder and expand the CSharp or Python folder depending on your language preference and the qna-app folder it contains. Each folder contains the language-specific files for an app into which you're you're going to integrate Azure AI Language question answering functionality.

Right-click the qna-app folder containing your code files and open an integrated terminal. Then install the Azure AI Language question answering SDK package by running the appropriate command for your language preference:

C#:

dotnet add package Azure.AI.Language.QuestionAnswering
Python:

pip install azure-ai-language-questionanswering
In the Explorer pane, in the qna-app folder, open the configuration file for your preferred language

C#: appsettings.json
Python: .env
Update the configuration values to include the endpoint and a key from the Azure Language resource you created (available on the Keys and Endpoint page for your Azure AI Language resource in the Azure portal). The project name and deployment name for your deployed knowledge base should also be in this file.

Save the configuration file.

Add code to the application
Now you're ready to add the code necessary to import the required SDK libraries, establish an authenticated connection to your deployed project, and submit questions.

Note that the qna-app folder contains a code file for the client application:

C#: Program.cs
Python: qna-app.py
Open the code file and at the top, under the existing namespace references, find the comment Import namespaces. Then, under this comment, add the following language-specific code to import the namespaces you will need to use the Text Analytics SDK:

C#: Programs.cs

csharp
// import namespaces
using Azure;
using Azure.AI.Language.QuestionAnswering;
Python: qna-app.py

python
# import namespaces
from azure.core.credentials import AzureKeyCredential
from azure.ai.language.questionanswering import QuestionAnsweringClient
In the Main function, note that code to load the Azure AI Language service endpoint and key from the 
