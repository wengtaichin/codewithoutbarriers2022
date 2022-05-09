# Microsoft Code Without Barriers 2022 Hackathon

## Inspiration
For this hackathon, I decided to apply what I learned into **General Problem Statement #2: Automating Invoice Reading**.
The main reason I am going with this Problem Statement is because I can resonate with the challenges faced by Wide World Importers. Those're exactly the same challenges faced by the Finance team. They receive tons of invoices from vendors, suppliers and even contract workers every month. They spend a lot of time just to extract the critical information from the invoices and manually enter into our Finance application for payment processing. As we know, this is a very manual process and can be prone to human errors. So I wish to leverage on Azure AI services to help the Finance team, at least to reduce the amount of time they need to spend on a monthly basis. Last but not least, I hope my solution "easyinvoicesubmission" can increase their productivity so everyone is happy at work. As simple as that.


## What it does
"easyinvoicesubmission" automates the process flow starting from invoice submission, invoice ingestion, invoice acknowledgement, invoice processing until high threshold invoice alert. 


## How we built it
I would suggest a role play for this demo. There should be 4 roles:
1. External Parties: Submit invoice 
2. Company Representatives: Acknowledge the invoice submission
3. Finance Colleagues: Process invoice
4. Finance Management: Be alerted on invoice with high threshold

Step 1: External Parties
_URL: dropbox.com_
_Login ID: wengtai_2022@outlook.com_
_Password: Microsoft2022!_

_URL: onedrive.live.com_
_Login ID: wengtai_2022@outlook.com_
_Password: Microsoft2022!_

Usually, external parties are the ones who submit invoices. So in terms of security, "easyinvoicesubmission" does not allow them to access Wide World Importers' internal applications. Hence the first step for them is to upload the invoices (image or pdf format) into a DropBox application under the folder "Invoices Submission". Once the upload is completed (the trigger), the file will be ingested into One Drive under the folder " hackathon2022" for Wide World Importers staffs to process (from this point onwards will be internal applications). The trigger is set to check every 3 minutes.


Step 2: Company Representatives
_URL: outlook.com_
_Login ID: mshackathon_1@outlook.com_
_Password: Microsoft2022!_

Once the invoice ingestion is completed, Company Representatives will be notified on a new invoice submission. However, at this point in time, there is no analysis on the invoice details yet as this is purely a new invoice submission notification email. The invoice file will be attached into the email.

After the invoice submission email is sent, Azure AI services will start to analyze the invoice using Form Recognizer API v2.1 Prebuilt Model. There will be 2 outcomes: Successful and Unsuccessful. 

Let's start with the Unsuccessful path. Unsuccessful is defined when the analyzing has timed out, is skipped or has failed. So when either one of the 3 status occurs, Company Representatives will be sent an email notification to request a clearer version of invoice from the external party. The invoice file will be attached into the email so Company Representatives know who to follow up with. The process for Unsuccessful path stops here.


Step 3: Finance Colleagues
_URL: outlook.com_
_Login ID: mshackathon_2@outlook.com_
_Password: Microsoft2022!_

Next, lets focus on the Successful path. Successful is defined when the analyzing is successful. However, there is a chance that "easyinvoicesubmission" is not confident on the results. So there is a condition to check on the invoice quality. There are 4 confidence values that are imperative to check on: Vendor Name, Invoice Date, Invoice Amount and Amount Due. All 4 values should pass the 90% confidence level. If either one of the 4 values does not pass the 90% confidence level, Finance Colleagues will be sent an email notification to manually verify the invoice details before processing the invoice. However, in the ideal scenario where all 4 values pass the 90% confidence level, Finance Colleagues also will be sent an email notification to proceed with processing the invoice.


Step 4: Finance Management
_URL: outlook.com_
_Login ID: mshackathon_3@outlook.com_
_Password: Microsoft2022!_

"easyinvoicesubmission" will continue to determine if either the Invoice Amount or Amount Due is greater than 1000 as that is the threshold set for categorized as high threshold invoice. For invoice which meets the criteria, Finance Management will be alerted with an email notification. It is just a FYI email and no action is required from Finance Management. For invoice with low threshold value, the process stops at Finance processing and for invoice with high threshold value, the process stops at Finance Management alert.

That concludes the process for "easyinvoicesubmission".


## Challenges we ran into
Challenge #1: How to Implement
This is my first time actually using Azure services to develop a solution. The certifications gained (AI-102 & AI-900) are more towards the theoretical and coding part but not until an end to end solution design. Honestly I struggled a lot during this hackathon. At the beginning I knew I need to use Azure Form Recognizer services but the "how to implement" part is the killing part. Like I knew what to use but I didn't know how to use it. Also I didn't know how to implement the process flow required to send email notifications. Google is my best friend to go to, I seriously did a lot of Google-ing and finally found out that combining Azure Logic Apps and Azure Form Recognizer would hopefully help to solve the challenges. 

Challenge #2: Custom Model
I tried training a custom model to extract the important entities by using some sample invoices I found on the Internet. However, the results is not too promising as the confidence level is way below 50%. So I decided to scrap my custom model and use the pre-built model. Anyway I still think that training a custom model is a good learning for myself. 

## Accomplishments that we're proud of
I am really proud of my solution "easyinvoicesubmission" even though it is simple and of course can always be improved. At least I solve some of the challenges faced by most companies and hopefully including Wide World Importers:

1. Central repository for invoice submission
Some of the external parties might submit in hard copy and company staffs need to do scanning for record keeping purpose, which is a non-productive task with hours wasted. Also for those who submit e-copy, they might email to the company staffs they know but might not be the right person to send to. This can be a big chaos when it comes to locating the actual invoice file and the actual invoice submission date because Finance colleagues would not know who submit the invoice, when do they submit the invoice and where are the invoices. Besides, this can also cause some of the company staffs' email to be bombarded with invoices email every single day. So with "easyinvoicesubmission", external parties do not need to know any person from the company to email the invoices to. All they need to do is to save the DropBox URL link and drop any invoice into that link anytime they want. As simple as that.

2. Time Saved for More Productive Tasks
With "easyinvoicesubmission", Finance colleagues do not need to read every single invoice anymore once they are confident with the results of the solution. Time is saved for more productive tasks such as ensuring payment is paid on time, doing Business Intelligence for Management on critical Finance KPIs etc.

3. Auto Email Notification
Using manual method of reading invoices, company staffs might overlook on sending email notification to the relevant parties, which is also a non-productive task with time wasted. However with "easyinvoicesubmission", there is no more such worry! Let "easyinvoicesubmission" do all the heavy lifting jobs, 24/7 throughout the year. 


## What we learned
Designing a solution from scratch can be overwhelming, but with the help of Azure AI services, it helps to simplify some of the tasks. At least I do not need to write lines of codes to extract the important entities from the invoices, all I need to do is just to use the pre-built model (with the condition I know how to use it). Any task might look daunting at the first place, but I believe that with continuous perseverance and never give up spirit, the journey can be rewarding in the end. 


## What's next for easyinvoicesubmission
I wish I have more time to improvise this solution. There are 2 main things I had in mind but due to time and knowledge constraint, they are not implemented in the solution yet. 

Idea #1: Include Analyzing Receipt Model
I actually want to include another model for analyzing receipt. So the idea is to create another folder in DropBox called "Receipt Submission". Then I would create another Logic Apps to process the receipts and apply similar process flow like invoice submission.

Idea #2: Save the Processed Data into Flat File 
I also intend to save the processed data analyzed by the prebuilt model into a flat file such as CSV or JSON. Everyday there will be a cut off time (assuming 6pm) where invoices submitted and data analyzed by "easyinvoicesubmission" before 6pm would be compiled. This file will be auto uploaded to the internal Finance application (e.g. SAP FICO module) so Finance can pay the external parties on time. All data is automatically filled in to the internal Finance application and no manual intervention is required to minimize human errors. However, Finance has the right to manually overwrite or change any of the value should there be such a need.
