# Alibaba Cloud SMS Integration Guide

Log in to Alibaba Cloud Console and enter "SMS Service" page: https://dysms.console.aliyun.com/overview

## Step 1: Add Signature
![Steps](images/alisms/sms-01.png)
![Steps](images/alisms/sms-02.png)

After the above steps, you will get a signature. Please write it into the Control Console parameter, `aliyun.sms.sign_name`

## Step 2: Add Template
![Steps](images/alisms/sms-11.png)

After the above steps, you will get a template code. Please write it into the Control Console parameter, `aliyun.sms.sms_code_template_code`

Note, signatures need to wait 7 business days for operator approval before they can be sent successfully.

Note, signatures need to wait 7 business days for operator approval before they can be sent successfully.

Note, signatures need to wait 7 business days for operator approval before they can be sent successfully.

You can wait for the approval to succeed, then continue with the following operations.

## Step 3: Create SMS Account and Enable Permissions

Log in to Alibaba Cloud Console and enter "Access Control" page: https://ram.console.aliyun.com/overview?activeTab=overview

![Steps](images/alisms/sms-21.png)
![Steps](images/alisms/sms-22.png)
![Steps](images/alisms/sms-23.png)
![Steps](images/alisms/sms-24.png)
![Steps](images/alisms/sms-25.png)

After the above steps, you will get access_key_id and access_key_secret. Please write them into the Control Console parameters, `aliyun.sms.access_key_id`, `aliyun.sms.access_key_secret`

## Step 4: Enable Mobile Registration Function

1. Normally, after filling in all the above information, you should have this effect. If not, you may be missing a step

![Steps](images/alisms/sms-31.png)

2. Enable allowing non-administrator users to register by setting parameter `server.allow_user_register` to `true`

3. Enable mobile registration function by setting parameter `server.enable_mobile_register` to `true`
![Steps](images/alisms/sms-32.png)

