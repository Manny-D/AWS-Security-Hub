# AWS Security Hub

![AWS_Security_Hub Header Image](https://github.com/Manny-D/AWS-Security-Hub/assets/99146530/0c72fe49-8f65-4152-bb4e-40375a889e24)

<br>

## Description

This lab was part of an online course that provided an overview of [Amazon Web Services Security Hub](https://aws.amazon.com/security-hub/). In the end, we assessed specific Findings and created a Custom Action that allowed us to send a notification to our Security Operations Center (SOC) team for remediation. 

AWS Security Hub is a cloud security posture management (CSPM) service that performs security best practice checks, aggregates alerts, and enables automated remediation.

<br>

## Summary

Upon accessing AWS Security Hub, we are presented with the summary page where the findings of enabled security standards are displayed.

![Summary](https://github.com/Manny-D/AWS-Security-Hub/assets/99146530/0d6d5e24-da2d-4ea0-b04c-1a2d044b50d9)

Security Hub continuously checks our resources against security best practices from enabled standards. The Summary page shows security scores for each standard and a combined score reflecting how well our configuration meets those standards.

If we click on one of the security standards, we will be presented with more details. <br>
eg. <b>AWS Foundational Security Best Practices v1.0.0</b>

![Standard overview](https://github.com/Manny-D/AWS-Security-Hub/assets/99146530/cc5a8102-00ba-437d-b6ef-77a8f3814780)

We can see why the <b>Compliance Status</b> is showing as <b>FAILED</b> by clicking on the hyperlinked <b>Title</b>.

![Finding specifics](https://github.com/Manny-D/AWS-Security-Hub/assets/99146530/87193d4e-f82d-4dbc-a6e8-2b0d01f8d94e)

Along with the additional details, recommended remediation instructions are provided!

<br>

## Insights

Security Hub displays several related findings here, which help to assess our security posture and quickly identify risks. 

![AWS Insights](https://github.com/Manny-D/AWS-Security-Hub/assets/99146530/f7c83c8e-b765-4a25-8f59-f1db920e00a3)

<br>

## Integrations

Security Hub integrates with various providers (3rd party and AWS services) that allow the import and / or export of findings. It normalizes and aggregates the ingested data and acts as a single pane of glass to help understand the security and compliance posture of our AWS account(s). 

To check on what integrations are supported, on the left pane, click on <b>Integrations</b>. 
You can search for an integration or scroll down the list. Then click on <b>Accept findings</b>. 

<b>Note</b>: for managed services, many AWS services will already be enabled to accept findings. For 3rd party providers, some may need to be purchased and require configuration prior to being able to Accept findings. 

![AWS Integrations](https://github.com/Manny-D/AWS-Security-Hub/assets/99146530/9545061e-3ce8-4810-9f86-2f0bfabf864a)

<br>

## Findings

This is where Security Hub will report any failed security checks and security issues that are detected across our AWS resources. 

![AWS Findings](https://github.com/Manny-D/AWS-Security-Hub/assets/99146530/2a2aa2f4-129a-49fa-a5f8-fcf280018460)

From here, the security team will need to decide how to take action. For the purpose of this lab, we’ll be setting up a custom action that allows us to notify the Security Operations Center (SOC) on selected findings. 

<br>

## Settings
 
On the left pane, click on <b>Settings</b> -> <b>Custom actions</b>.

![Custom actions](https://github.com/Manny-D/AWS-Security-Hub/assets/99146530/df907ecb-328d-4f31-923b-74846142119d)


In the pop-up, enter in an <b>Action name</b>, <b>Description</b> and <b>Custom actin ID</b> -> then click <b>Create custom action</b>.

![Create custom action](https://github.com/Manny-D/AWS-Security-Hub/assets/99146530/5600cbaa-9522-48d3-91b4-b648c00461c0)


It will now be listed.   

![Created custom action](https://github.com/Manny-D/AWS-Security-Hub/assets/99146530/e705b052-47ab-4bca-b9f1-cd700b4aee64)

We now need to link this Custom action to an Event Bridge rule.

<b>Note</b>: Be sure to copy the Custom action ARN, since it will be needed for the next step!

<br>

### Amazon Event Bridge

Navigate to <b>Amazon Event Bridge</b> -> click <b>Create rule</b>.

![Amazon Event Bridge](https://github.com/Manny-D/AWS-Security-Hub/assets/99146530/9696b1d6-370d-40c1-bdf2-6aba07a472da)

For the <b>Define rule detail</b> options under <b>Rule detail</b>, enter a <b>Name</b> while leaving the other settings at their default -> click <b>Next</b>.

![Define rule detail](https://github.com/Manny-D/AWS-Security-Hub/assets/99146530/b9dff249-54cb-4002-a870-e8bbd5d1dedb)

For the <b>Build event pattern</b> options under <b>Event source</b>, leave the <b>AWS events or EventBridge partner events</b> radio button ticked.

![Event source](https://github.com/Manny-D/AWS-Security-Hub/assets/99146530/debfbca5-d5b7-4ba2-b2df-cd2a2fbb6e72)

Scroll down to <b>Event pattern</b> -> select the following:
- <b>AWS service</b>: <b>Security Hub</b>
- <b>Event type</b>: <b>Security Hub Findings - Custom Action</b>
- <b>Specific custom action ARN(s)</b>: (paste in the <b>custom action ARN</b> copied earlier)

![Event pattern](https://github.com/Manny-D/AWS-Security-Hub/assets/99146530/f8ac9efa-9abd-4e46-afbb-9580824d2168)

then click <b>Next</b>.

For the <b>Select target(s)</b> options under <b>Target 1</b>, select the following:
- <b>Target types</b>: leave the <b>AWS service</b> radio button ticked 
- <b>Select a target</b>: search for / select (Simple Notification Service) <b>SNS topic</b> 
- <b>Topic</b>: <b>SOC1Team</b>

![Select targets](https://github.com/Manny-D/AWS-Security-Hub/assets/99146530/4413aa0f-c227-48a2-a517-d3f8393d21da)

then click <b>Next</b>.

![Create rule](https://github.com/Manny-D/AWS-Security-Hub/assets/99146530/93b28d11-d556-464e-bb78-4c9a746f88df)

We can skip the <b>Configure tags</b> options -> click <b>Next</b> and on the <b> Review and create</b> options, scroll to the bottom and click <b>Create rule</b>.

![EventBridge Rules](https://github.com/Manny-D/AWS-Security-Hub/assets/99146530/a52ea06e-e71d-44b3-b0b9-3b2ce9e24387)

Once completed, it can be located on the left pane under <b>Events / Rules</b>.

<br>

### Testing

Now to confirm the Custom action is working, navigate to <b>Findings</b> and to the left of any Finding, place a check mark in the empty box. 

![AWS Findings 2](https://github.com/Manny-D/AWS-Security-Hub/assets/99146530/19b7e5bc-1ffc-4dad-b6a7-c21aa88daccc)

To the top right, click on <b>Actions</b> -> select the <b>Custom action</b> created earlier.

![Actions](https://github.com/Manny-D/AWS-Security-Hub/assets/99146530/b9396dc0-3cd4-4da8-a7b3-fd9ccfa1ba36)

A few moments later, the following banner should appear:

![Sent finding](https://github.com/Manny-D/AWS-Security-Hub/assets/99146530/de7bc07e-f557-4630-880f-a5ecfb829408)
