# AWS Security Hub

![AWS_Security_Hub Header Image](https://github.com/Manny-D/AWS-Security-Hub/assets/99146530/0c72fe49-8f65-4152-bb4e-40375a889e24)

<br>

## Description

This lab was part of an online course that provided an overview of [Amazon Web Services Security Hub](https://aws.amazon.com/security-hub/), looked at specific Findings and created a Custom Action that allowed us to send a notification to our Security Operations Center (SOC) team. 

AWS Security Hub is a cloud security posture management (CSPM) service that performs security best practice checks, aggregates alerts, and enables automated remediation.

<br>

## Integrations

Security Hub integrates with various providers (3rd party and AWS services) that allow the import and / or export of findings. It normalizes and aggregates the ingested data and acts as a single pane of glass to help understand the security and compliance posture of our AWS account(s). 

To check on what integrations are supported, on the left pane, click on <b>Integrations</b>. 
You can search for an integration or scroll down the list. Then click on <b>Accept findings</b>. 

<b>Note</b>: for managed services, many AWS services will already be enabled to accept findings. For 3rd party providers, some may need to be purchased and require configuration prior to being able to Accept findings. 

![AWS Integrations](https://github.com/Manny-D/AWS-Security-Hub/assets/99146530/9545061e-3ce8-4810-9f86-2f0bfabf864a)

<br>

## Findings

Here is where Security Hub will report any failed security checks and security issues that are detected across the AWS resources. 

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
