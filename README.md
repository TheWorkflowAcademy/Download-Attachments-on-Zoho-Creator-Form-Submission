# Download-Attachments-on-Zoho-Creator-Form-Submission
A Deluge script that lets you download any attachments on submission of a Zoho Creator form.

## Core Idea
Suppose you have a Zoho Creator form with a File Upload/Image/Signature field. When a user submits the form, you need download the attachment and upload it to Zoho CRM, Zoho WorkDrive or send it as an attachment via email.

## Configuration
* Zoho OAuth Connections needed:
  * ZohoCreator.report.READ
  * WorkDrive.files.ALL (for uploading to WorkDrive)

## Tutorial

### 1. Get the Creator Form ID
Have you ever realized that if you name a field "ID" on a Zoho Creator form, the API name will automatically be changed to "ID1"? That is because "ID" is a Zoho system variable. In order to download an attachment from a Creator form, we first need the Creator Form record ID. A simple `input.ID` command gets you the form record ID.
* Note: This only works for the successful submission workflow.

```javascript
createrID = input.ID;
```

### 2. Download the Attachment
Once you have the Creator form record ID, use the API call below to download the attachment in your script.
```javascript
resp = invokeurl
[
	url :"https://<base_url>/api/v2/<account_owner_name>/<app_link_name>/report/<report_link_name>/<record_ID>/<field_link_name>/download"
	type :GET
	connection:"<creator_connection>"
];
info resp;
```

### 3. Upload the Attachment
At this point, you already have the attachment downloaded. You can now choose to upload it to CRM, WorkDrive or even send it as an email attachment.

* Upload to Zoho WorkDrive Folder
```javascript
<response> = zoho.workdrive.uploadFile(resp, <folder_id>, <file_name>, <override_name_exist>, <workdrive_connection>);
```

* Attach to Zoho CRM Record
```javascript
attach = zoho.crm.attachFile(<module_name>,<record_id>,resp);
info attach;
```

* Send Email as Attachment
```javascript
sendmail
[
	from: <from_address>
	to: <to_address>
	cc: <cc>
	bcc: <bcc>
	reply to: <reply_to_address>
	subject: <subject>
	message: <message>
	attachments: file: resp
]
```
