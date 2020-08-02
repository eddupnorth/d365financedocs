---
layout: 
title: Automating the Vendor Onboarding Process

orig-auth: Paul Dipankar
orig-url: https://pauld365.home.blog/2019/09/29/leverage-your-vendor-on-boarding-with-microsoft-azure-graph-form-flow-dynamics-365-fin-ops/
---


# Business story

It’s a great story of Paul & Associate from South East Asia a Parts Retailer of BMW & Audi cars. Customer is managing 6000 over vendors for their 20 plus legal entities. Managing vendor and on-boarding them with a decentralized system for 20 legal entities very cumbersome. Mr. Jonny, CEO, need a solution to handle this situation, he wants to leverage all the Microsoft Technology Stage in cloud to solve this problem.

# How to solve the problem

![Vendor Onboarding Flow](/images/vendoronboard01.jpg "Vendor Onboarding Flow") 

## Create a form in Microsoft Forms Pro

This generates a form from which you can distribute (e.g. embedding on your website, emailing link to prospective vendors etc. ).

### How to create form.

1. Open you browser and type : forms.office.com
2. Form will open.
3. Create a new form click the create new form option.
4. New form creation page will open
5. Put the form titled “I am interested supply goods and services to Paul & Associates”, and enter the description.
6. For adding your questions click add new. Now we create the all information which you want to capture during vendor interest. In the form you can create one by one.

For adding any new fields you have to determine the field type. Add the following fields:

| No.    |  Question        |
| ------ |  ------          |
| 1      |  What is your organization name? |
| 2      |  Do you have any Organization number? |
| 3      |  What is your business address? |
| 4      |  Contact person First Name |
| 5      |  Contact person Last name |
| 6      |  Contact number of your organization |
| 7      |  Contact email ID |
| 8      |  Why you want to do business with us and what you want to supply us? |

7. Let’s put a nice theme on our form
8. Let’s check the form and preview it.
9. Click the preview button

If its look good in preview, put the data and see the form responds. All good, put the link in you company website.

### Configure flow and integrate with Team & Dynamics 365 Finance & Operation

1. Open the browser and write https://preview.flow.microsoft.com. Make sure you are in correct CDS data base. Make sure you have sign in with same office 365 id with Microsoft Form Pro.
2. Microsoft Flow will be open, click the connector.
3. Search the connector called Microsoft form
4. Select the Microsoft form pro, click on the connector, new flow trigger from will open as below.
5. Click “when a new response is submitted”, new flow canvas will open. 1st Define the name of the flow and then select the Form ID from drop down list.
6. Click new step
7. Choose an action, search and select the “Microsoft Form”
8. Select “Get respond details”
9. Upon select the get response details, flow will ask where from you need to get the respond. So select the proper form id which we created in the form “ I am interested supply goods and services to Paul & Associates”
10. Select the respond id from the “Dynamic content”. Dynamics content will show click you left mouse in the respond id field.
11. Now our form submission respond id configure, here is the configuration look like;
12. Now I would like to add few more action, I want a notification in my email id when anyone submit an interest.
13. Click add an action, and select “Mail” action,
14. Select the action “send an email notification”
15. Upon selection some agreement acceptance is required, please accept the same and then defile the below detail like , whom to send the emails, what is subject and what is the email body. I have used some dynamic content.
16. Now I am going to integrate this interested respond with Dynamics 365 Finance & Operation. So, doing this again I will add an action, search connector “Dynamics 365 for finance & operation”.
17. Select the action call “Create record”. This action will transform the form respond which submitted by an interested vendor who want to do business with our organization via Dynamics 365 Fin Ops data entity in “Prospective vendor registration request”.
18. Define you D365 Fin Ops server name and select the data entity. Data entity you select 

“ProspectVendorRegisterationRequests”

Moment you complete both the step , action “create record” will be enlarged and it will open many Items for mapping.
19. Now we are going to do the mapping with our form fields with D365 FinOps data entity fields.
Note: I put by default company “USMF” & Language “En-us”. Its not mandatory.
20. Again, I will add one more action “Mail” to inform submitter “Thanks for your interest, we have received your detail. Our team will do the review as needed they will call you to discuss more”
21. Save the flow

Note: Due to my organization policy I will not execute the email steps

### Create a new enterprise application in Azure

1. Open the browser and write https://portal.azure.com . Make sure you have full administrator right in Azure portal on respective subscription.
2. Once login to azure portal. Select the “Azure Active Directory – > App Registration -> New Registration

3. Click New Registration and define the below detail

Given a name of the application. Example “AzureB2BGraph”

Select the “account in any organizational directory (Any Azure AD Directory – Multitenant) and person Microsoft account

Redirect URL (Optional): put your D365 Fin & Ops server details

4. Your app created, now you need to make sure you have all the required data which you need to setup and copy and keep safe place which needed later stage for setup in D365 Fin&Ops. Copy the “Application (Client) ID” of the application in a notepad.
5. Now let’s move to “API Permission” and click “add permission”
6. Add Permission -> select Microsoft Graph
7. Click on graphs and give application permission and delegates permission
8. Give blow permission in application and delegates, this you need find it manually from the list.
9. Grant the consent
10. Please check all the application permission and delegate permission are below status with green icons.
11. Create secrets key for this app. (Certificate & Secrets -> Client Secrets -> New client secrets)
12. Define a name and select never expire
13. Add the same and copy the secret key in your notepad where you keep the application (client) id.
14. Save the app and log out from your Azure portal.

### Configure Dynamics 365 Finance and Operation

We need to do many setups before we run the process from Microsoft Form to D365 Finance & Operation for vendor onboarding.

#### B2B Invitation Configuration

1. System administration module -> Setup -> B2B Invitation Configuration

2. Click Edit -> Enabled (yes)

3. Define the client ID (Application Client ID which you have in your notepad which you collected during your Azure app registration)

4. Define secret id / application key (secret ID which you have in your notepad which you collected during your Azure app registration)

#### User creation workflow configuration

1. System administration module -> Workflow -> User workflow -> Create new

Make sure these two workflows configure properly

2. Here is the configuration of “User request workflow (Platform)”, this workflow will execute B2B user creation process. ( Submit – > send a invite to prospective vendor -> Vendor accept and fill up the date -> approval (my case auto action) -> Provision Azure AD B2b user -> Automatic Provision -> Notify the user)

Also configure the user request workflow

#### External users configuration

System administration module -> Security -> External Role ( Make all the role setup as below)

#### Procurement & sourcing Parameter configuration

1. Procurement & Sourcing Module -> Setup -> Procurement & Sourcing Parameter -> Vendor collaboration tab
2. Auto submit prospective vendor registration to workflow – > Yes
3. Define the "Default company"
4. As needed select other data in the form.

#### Procurement & sourcing workflow configuration  

1. Procurement & Sourcing Module -> Setup -> Procurement & Sourcing workflow -> New -> configure “vendor add application workflow”
2. Here is my configuration, single approval


# Vendor on-boarding execution

Scenario 1: Mr. Jim Ho Gao, Sales Director, Gao Lim JV Pte Ltd, a parts manufacturer, based in Singapore, who want to do business with Paul & associate. Mr. Jim Ho Gao was browsing Paul & Associate website and found the link for show the interest. He fills up his organization details, conduct details etc.

Scenario 2: Mr. Jim Ho Gao interest received by Mr. Paul, Operation head of the organization. Who was looking for great supplier in South East Asia region to support their business. Mr. Paul, given a call to Mr. Gao, to understand how they do business and find Gao Lim JV Pte Ltd can be a potential vendor. Mr. Paul Process the “Prospective vendor registration request“ as asked his IT team to process B2B business invite to Gao Lim JV Pte Ltd.

Scenario 3: Mr. Jim Ho Gao, got the B2B invite from Paul & Associates. He accepts the invite and filled up the required detail as requested by Paul & Associate.

Scenario 4: Mr. Paul has been informed by his IT team that “Gao Lim JV Pte Ltd” user has been created and a notification has been send to vendor. Mr. Paul, process final vendor creation request to his finance department. Mr. Ram got the request for final review in finance department, he approved vendor request and vendor got created. Upon vendor creation Mr. Paul got a notification that vendor got created in the system.

Scenario 5: Mr. Jim Ho Gao, got the information about the successful vendor registration with Paul & Associates. He checked the access via given link by Paul & Associates. 

Scenario 1: Mr. Jim Ho Gao, Sales Director, Gao Lim JV Pte Ltd, a parts manufacturer, based in Singapore, who want to do business with Paul & associate. Mr. Jim Ho Gao was browsing Paul & Associate website and found the link for show the interest. He fills up his organization details, conduct details etc.

Assumption: Mr. Gao, find the form link in the website.

1. Open the form
2. Start fill up the data
3. All the data filled up now time for submit
4. Let’s check the responds submitted or not.
5. Flow run other side, run the process in the back and create the data D365 FinOps, here is the flow run status (You can go to flow -> find your flow which you configure and check all run.
6. Prospective vendor registration request created in D365 FinOps ( Procurement & Sourcing Module -> Vendor -> Vendor Collaboration -> Prospective vendor registration request )
7. Confirmation email send to Jim Ho Gao ,

Scenario 2: Mr. Jim Ho Gao interest received by Mr. Paul, Operation head of the organization. Who was looking for great supplier in South East Asia region to support their business. Mr. Paul, given a call to Mr. Gao, to understand how they do business and find Gao Lim JV Pte Ltd can be a potential vendor. Mr. Paul Process the “Prospective vendor registration request“ as asked his IT team to process B2B business invite to Gao Lim JV Pte Ltd.

1. Mr. Paul received the notification about Mr. Gao Interest. Mr. Paul given a call and understand what Mr. Gao want to offer. Mr. Paul decided to move forward with Mr. Gao company.
2. Mr. Paul want to invite Mr. Gao’s company B2B invite where Gao Lim JV Pte Ltd, other details can be submitted for further evaluation process.
Invite user (Procurement & sourcing module – > Vendor -> Vendor Collaboration -> Prospective vendor registration request)
3. Invite use will trigger a workflow (User request [Platform])
4. Let me check the workflow status, what is happening there. (Click -> Prospective vendor user request) – > It will take you user request form. Its showing Azure AD B2B user invite pending to send. IT team going to complete the process as per workflow setup.
5. Approval given by IT Admin, upon approval invite will send to Mr. Gao

Scenario 3: Mr. Jim Ho Gao, got the B2B invite from Paul & Associates. He accepts the invite and filled up the required detail as requested by Paul & Associate.

Lets check Mr. Gao mail box and see what is there.

1. Open Outlook.com
2. Put Mr. Gao email id which he provided last time in the interest form.
3. Mr. Gao got the invite
4. Mr. Gao click -> Get started (Note: it will ask for permission . just click accept)
5. Start fill the data as wizard direct.
6. Click next
7. Define the country code (via search), select and next
8. Click acknowledge, then Next
9. Define the address, telephone, email and fax etc., then click next.
10. Define the contact person detail, then click next
11. Define other business informs line Terms of payment, currency, tax exemptions etc. Then click Next.
12. Add the procurement categories which Mr. Gao want to sell to Mr. Paul’s company, (…) click there you can see the procurement categories. Once added click Next.
13. Click the finish,

Now Mr. Gao need to wait for Mr. Paul. Once vendor registration approved, Mr. Gao can able to access the vendor portal.

Scenario 4: Mr. Paul has been informed by his IT team that “Gao Lim JV Pte Ltd” user has been created and a notification has been send to vendor. Mr. Paul, process final vendor creation request to his finance department. Mr. Ram got the request for final review in finance department, he approved vendor request and vendor got created. Upon vendor creation Mr. Paul got a notification that vendor got created in the system.

IT team has been informed Mr. Paul, “Gao Lim JV Pte Ltd” User ID created in system via B2B process. This is took place when IT process the invite.

Mr. Paul, now can process the final request for create vendor,

1. Procurement & sourcing -> Vendor -> Vendor collaboration -> Vendor request.
Vendor request already create once Mr. Gao finish his info submission.
2. Open the request and check the details and submit.
3. Mr. Ram got the request and he will fill up other details like vendor group etc and approved the request.
4. Request approved.
5. Vendor created in the system.

Scenario 5: Mr. Jim Ho Gao, got the information about the successful vendor registration with Paul & Associates. He checked the access via given link by Paul & Associates. 

Mr. Paul given a phone call and email to Mr. Gao, about successful vendor registration. Now Mr. Gao can access vendor portal of Mr. Paul’s company.

1. Try the link which has been provided by Paul & Associate Inc IT Team.
