# Control Console Volcano Dual-Stream Speech Synthesis + Voice Cloning Configuration Tutorial

This tutorial is divided into 4 phases: Preparation Phase, Configuration Phase, Cloning Phase, and Usage Phase. Mainly introduces the process of configuring Volcano dual-stream speech synthesis + voice cloning through the Control Console.

## Phase 1: Preparation Phase
Super administrators should first complete opening the Volcano Engine service and obtain App Id and Access Token. Volcano Engine will give one voice resource by default. This voice resource needs to be copied into this project.

If you want to clone multiple voices, you need to purchase and open multiple voice resources. Just copy each voice resource's sound ID (S_xxxxx) into this project. Then assign them to system accounts for use. Detailed steps are as follows:

### 1. Open Volcano Engine Service
Visit https://console.volcengine.com/speech/app, create an application in application management, check speech synthesis large model and voice replication large model.

### 2. Get Voice Resource ID
Visit https://console.volcengine.com/speech/service/9999, copy three items: App Id, Access Token, and Sound ID (S_xxxxx). As shown in the figure:

![Get Voice Resource](images/image-clone-integration-01.png)

## Phase 2: Configure Volcano Engine Service

### 1. Fill in Volcano Engine Configuration

Use super administrator account to log in to the Control Console, click `Model Configuration` at the top, then click `Speech Synthesis` on the left side of the model configuration page, search for "Volcano Dual-Stream Speech Synthesis", click modify, fill in your Volcano Engine's `App Id` into the `Application ID` field, and fill in `Access Token` into the `Access Token` field. Then save.

### 2. Assign Voice Resource ID to System Accounts

Use super administrator account to log in to the Control Console, click `Voice Cloning` and `Voice Resources` at the top.

Click New button, select "Volcano Dual-Stream Speech Synthesis" in `Platform Name`;

Fill in your Volcano Engine voice resource ID (S_xxxxx) into `Voice Resource ID`, press Enter after filling in;

In `Owner Account`, select the system account you want to assign to. You can assign it to yourself. Then click Save

## Phase 3: Cloning Phase

If after logging in, clicking `Voice Cloning` → `Voice Cloning` at the top shows `Your account currently has no voice resources, please contact administrator to assign voice resources`, it means you haven't assigned the voice resource ID to this account in Phase 2. Then go back to Phase 2 and assign voice resources to the corresponding account.

If after logging in, clicking `Voice Cloning` → `Voice Cloning` at the top can see the corresponding voice list. Please continue.

In the list, you will see the corresponding voice list. Select one of the voice resources and click the `Upload Audio` button. After uploading, you can listen to the voice or extract a segment of the voice. After confirming, click the `Upload Audio` button.
![Upload Audio](images/image-clone-integration-02.png)

After uploading audio, you will see in the list that the corresponding voice will change to "Pending Replication" status. Click the `Replicate Now` button. Wait 1~2 seconds for the result.

If replication fails, hover the mouse over the "Error Information" icon to see the failure reason.

If replication succeeds, you will see in the list that the corresponding voice will change to "Training Successful" status. At this time, you can click the Modify button in the `Voice Name` column to modify the voice resource name for easy selection and use later.

## Phase 4: Usage Phase

Click `Agent Management` at the top, select any agent, and click the `Configure Role` button.

Select "Volcano Dual-Stream Speech Synthesis" for Speech Synthesis (TTS). In the list, find the voice resource with "Cloned Voice" in the name (as shown in the figure), select it, and click Save.
![Select Voice](images/image-clone-integration-03.png)

Next, you can wake up Xiaozhi and have a conversation with it.

