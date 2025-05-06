# SOAR & EDR Automation: LimaCharlie, Tines, Slack & Gmail Integration


# Project Overview

This project serves as an extension of my initial [**LimaCharlie-Sliver Lab**](https://github.com/kyntrp/LimaCharlie-Sliver), where I explored endpoint telemetry and detection engineering. Building upon the foundational LimaCharlie setup, this phase integrates Security Orchestration, Automation, and Response (SOAR) using Tines. Additionally, automated alerting via Slack & Gmail enhances real-time incident visibility.
The goal of this lab is to streamline the cybersecurity workflow, demonstrating how EDR data can trigger automated playbooks for efficient threat response. This setup showcases a practical SOAR-driven EDR workflow tailored for automated security operations.



# Lab Setup Environment:


🔹 **Virtual Machines (VMs)**

•	Windows 10 (Infected Machine) – Target endpoint for LimaCharlie EDR testing. 




🔹 **Security Tools:**

•	LimaCharlie (EDR) - Endpoint telemetry, detection engineering, and response.

•	Tines (SOAR) – Security orchestration for automated workflows .

•	Slack & Gmail – Incident notification and response verification. 


![image](https://github.com/user-attachments/assets/42a0a394-a548-465d-98a9-811f307586e0)




# Steps:


In installation and setting up of LimaCharlie, I covered it in my previous lab [**LimaCharlie-Sliver Lab**](https://github.com/kyntrp/LimaCharlie-Sliver) but i will run it again here. it might have some changes cause I've done it a little long time ago and to also document the whole process of building this project.

Now got to Limacharlie webpage (https://app.limacharlie.io/) login your account or create an account if you don't have one.

create you organization. in my case, i will use my existing that i also use in my previous LimaCharlie project.


![image](https://github.com/user-attachments/assets/805bd39d-12e3-48b5-b9a9-3cba747d360b)

click 'Sensors' dropdown, select Installation Key then click 'Create Installation Key'. fill up the description.
You can also delete the other default installation key.

![image](https://github.com/user-attachments/assets/7c4f96ca-f35f-40c2-b629-0a7c41c0dcab)


In the same page, scroll down at the bottom till you get to 'Sensor Downloads'. In EDR section. select the operating system of your machine then copy the link and open it to the target PC. it will automatically download the sensor installer.

![image](https://github.com/user-attachments/assets/0f0a9973-fcb5-48ae-801b-44a656882697)


Once the download is complete. open the Windows Powershell in administrator.
navigate to Downloads directory (`cd C:\Users\vboxuser\Downloads`)

then type
`.\<insert filename of the downloaded file from LimaCharlie> -i <paste installation key here>`

![image](https://github.com/user-attachments/assets/07f36e9a-4978-4065-a9b2-9812b2b39877)


you can copy your installation in 'Installation Keys' section the click under the Sensor Key
![image](https://github.com/user-attachments/assets/3a3682a3-ab5f-41ac-aa04-f322f4ff3847)

If the installation is successful, you will receive something like this : 

![image](https://github.com/user-attachments/assets/53fadac4-e56f-4c93-a561-7f2f3053bc0d)


The machine should also appear in the 'Sensor List' 

![image](https://github.com/user-attachments/assets/88482a96-35f3-4a85-9a44-b91a4e88bfb6)


Click this :  [**LaZagne**](https://github.com/AlessandroZ/LaZagne)
then go to Release

![image](https://github.com/user-attachments/assets/8a44949c-e74b-4a94-8333-f01ed76e9f48)

then click the 'LaZagne.exe'
it will automatically blocked by Microsoft Defender so we need to disable it.
![image](https://github.com/user-attachments/assets/e0488de3-f079-440f-8b04-7c42b21933ee)

go to 'Windows Security' > 'Virus % threat protection'
![image](https://github.com/user-attachments/assets/1597a974-435f-40a5-84fe-f8dc3349947a)

under 'Virus & threat protection settings' click 'Manage settings' then disable the 'Real-time protection'
![image](https://github.com/user-attachments/assets/42359c26-4839-46a3-adeb-40c0f6309edb)

now click the three dot in the right of download notification then select 'Keep' another warning will appear saying that the app is unsafe, select 'Keep anyway'
![image](https://github.com/user-attachments/assets/b5fad5bd-886b-4cd4-81e6-5a17b3fb5644)  ![image](https://github.com/user-attachments/assets/cd9eb9ff-be0e-4a96-964d-bdf4cede08b0)

Once you completed to download the LaZagne.exe, open a powershell in administrator then start app.

![image](https://github.com/user-attachments/assets/bb0fdb69-bdfd-4059-9d96-eb988e0df157)


our LimaChalie should pickup the process for LaZagne. Let's check it out.
Go to 'Sensors List'. click our machine then go to 'Timeline' and search 'LaZagne'
click the event to see the details

![image](https://github.com/user-attachments/assets/46eaca47-8396-492a-86c0-19d23e462eda)


Now we will create a Detection & Response Rules
in your browser, open a new tab of LimaCharlie. Click your organization and head into 'D&R Rules' then click 'Add Rule'

![image](https://github.com/user-attachments/assets/ee51f1ad-5bb1-4a65-a5c2-7f9f80245537)


Paste this in Detect Box :


  ```
events:
  - NEW_PROCESS
  - EXISTING_PROCESS
op: and
rules:
  - op: is windows
  - op: or
    rules:
    - case sensitive: false
      op: ends with
      path: event/FILE_PATH
      value: LaZagne.exe
    - case sensitive: false
      op: contains
      path: event/COMMAND_LINE
      value: LaZagne
    - case sensitive: false
      op: is
      path: event/HASH
      value: '3cc5ee93a9ba1fc57389705283b760c8bd61f35e9398bbfa3210e2becf6d4b05'

```

Paste this in Response Box :
```
- action: report
  metadata:
    author: MyDFIR
    description: TEST - Detects Lazagne Usage
    falsepositives:
    - ToTheMoon
    level: high
    tags:
    - attack.credential_access
  name: #insert name  - HackTool - Lazagne

```


![image](https://github.com/user-attachments/assets/484efcde-6f24-49b9-a935-bd8987c68552)



In LimaCharlie, we can test the rules we've created to see if it will work or there is an error.
In the same page, scroll down at the very bottom then click 'Target Event'. You need to paste the code of event so we will go back to timeline and copy the whole event.

![image](https://github.com/user-attachments/assets/13e0f2cd-8717-4073-9fd0-5a4a846e0898)

after you done pasting the code. click the 'Test event' in the bottom.

![image](https://github.com/user-attachments/assets/a4bcf584-5eb2-4feb-8ab3-a23fbbe4a13f)



for more information about creating rules in LimaCharlie. you can check it here : https://docs.limacharlie.io/docs/detection-and-response


Now let's try to run the LaZagne.exe again to see if we can capture a detection. go to 'Detection' section to check what you captured.
![image](https://github.com/user-attachments/assets/754bd5b3-7d40-412a-86d6-e2fb249fb3ed)


Go to 'www.slack.com' then click 'GET STARTED'.
![image](https://github.com/user-attachments/assets/bb26eabe-43d1-4223-a49a-1ddead2086da)

create an account. make sure to input a valid email because it will send a verification.
once done creating account. create a channel.
In the left side. create a channel.
![image](https://github.com/user-attachments/assets/d15c17a2-abae-4d9a-9bfb-c22cfed3ac28)


Go to 'www.tines.com'
Sign up for account.

In your teams, Create a new story, this is where we will create our playbook (Note : make sure that you make new story on your team, not on personal)

![image](https://github.com/user-attachments/assets/3e9cb4aa-426a-4f3e-8260-328d2da8339b)


First, we have to establish a connection between LimaCharlie and Tines.
Drag the 'Webhook' in the board name it 'Retrieve Detection'

![image](https://github.com/user-attachments/assets/f29a17e2-4cb2-46bd-bd4d-7206168f51d9)


Copy the 'Webhook URL'. go back to LimaCharlie. Click your organization and go to 'Outputs'.
Click add 'output'.

![image](https://github.com/user-attachments/assets/e6e4333f-4347-491f-8633-9c226eccda19)

Select 'Detections'

![image](https://github.com/user-attachments/assets/faca27ed-61c8-4b8f-94a6-956116ed3f08)

Select 'Tines'

![image](https://github.com/user-attachments/assets/e4bbfb26-1b16-477e-8a3e-bb8c34ed1470)

Put a name then in 'Destionation Host', paste the Webhook URL copied earlier.

![image](https://github.com/user-attachments/assets/f9f8c17e-2963-49f6-9b4d-4300dc37d5bf)

to get a sample. retriger LaZagne.exe in your device again. then click 'Refresh Samples'.

![image](https://github.com/user-attachments/assets/784c5ace-5456-4505-8ccf-e83fcc580e89)

go back to Tines, in 'Webhook', click 'Events'.
![image](https://github.com/user-attachments/assets/792ef6ea-2d5e-4d8f-a382-9e3c0722ca73)


Looking back to our diagram, Everytime LimaCharlie send a detection in Tines, it will send the detail of the detection in Slack and Email.
Our next step is to create a link betweek Tines and Slack.
Go to Slack and take a look at some applications clicking the '3 dot' in the top left.

![image](https://github.com/user-attachments/assets/049f2b10-5d25-422f-bd5d-49c88795454d)

Select 'Automation' and search for 'Tines' then click 'Add' then 'Add to slack'

![image](https://github.com/user-attachments/assets/72a9f633-feee-4a4d-b732-76bd6d625a16)

Now, go back to the Tines dashboard. click template and search for 'Slack' then drag it to dashboard

![image](https://github.com/user-attachments/assets/b9bb7208-3494-44a1-82d1-8460863051d4)

select 'Send a message'
![image](https://github.com/user-attachments/assets/2029e68a-93f5-4e24-a8a1-1acdc8ef2997)

back to the channel of slack we've created for alerts. copy the 'Channel ID' at the bottom.
![image](https://github.com/user-attachments/assets/36a6ce17-516a-4494-ab6b-bb0d0049d52e)

paste it to tines slack template build 'Channel/User ID'
![image](https://github.com/user-attachments/assets/f914c6d9-eb43-4a8a-9e48-8d216b1461e9)


Hover the mouse in the 'Webhook' an 'arrow down' will appear. click it then drag to slacks to connect each other.
![image](https://github.com/user-attachments/assets/61e0e97d-352a-4663-b990-d26a6efb2684)

Next is the email, drag the 'Send Email' into the dashboard and connect it also to our webhook.
![image](https://github.com/user-attachments/assets/e050ec55-5b6a-483a-8f71-d3b08fa9b5e4)

in build section, fill up the fields.
test if it will work by clicking 'test' it will show the retrieved detections in our webhook, select the latest one. (you can also do the same in slacks to check if it will receive a message)

![image](https://github.com/user-attachments/assets/9fe90ca0-15c3-4ec2-a673-5980ad78c21a)


![image](https://github.com/user-attachments/assets/f8f2ee45-4401-41b7-8dec-e29ebf920725)



Let's edit the message that will be sent in slack and email. base on the diagram i created, the details to be sent is :

```
Time
Computer Name
Source IP
Process
Command Line
File Path
Sensor ID
Link to detection (if applicable)
```
Go to 'Webhook' then click Events. Select the latest one. in details of the event, dropdown the 'body'
![image](https://github.com/user-attachments/assets/e06cbd1a-cbd0-4f5b-a688-e5786e111be0)

Here is where we are getting the details we needed for the messages notification.
if you hover your cursor in the beginning of each line, an icon for copy will appear. just click it to copy the fields you need.(check the 'cat' code line)

![image](https://github.com/user-attachments/assets/22827d2d-c8ac-4df7-9849-a2b1d4b3bf06)

i will also put here all the fields needed to save your time (note : in some version,  it is <<retrieve_detections.body.cat>> instead of  <<retrieve_detection.body.cat>> so if it didn't work try putting or removing 's' in the 'detection' part)



```
Title  -   <<retrieve_detection.body.cat>>
Event Time   - <<retrieve_detection.body.detect.routing.event_time>>
Hostname  - <<retrieve_detection.body.detect.routing.hostname>>
Username -  <<retrieve_detection.body.detect.event.USER_NAME>>
Device IP - <<retrieve_detection.body.detect.routing.int_ip>>
File path - <<retrieve_detection.body.detect.event.FILE_PATH>>
Command Line -   <<retrieve_detection.body.detect.event.COMMAND_LINE>>
Sensor ID  - <<retrieve_detection.body.detect.routing.sid>>
Detection link -  <<retrieve_detection.body.link>>
```



Now that we have all the details we need. let's go back to slack to modify the message that will be sent to our alert channel.

![image](https://github.com/user-attachments/assets/b0c3a985-e334-4f3c-a43b-b9935e8b1116)


Let's test it again to see what it looks like in actual message. 

![image](https://github.com/user-attachments/assets/887f3064-5816-4b79-835e-1b730785802d)


Next, paste the same message in Email sender but this time we will put <br> every other line. 


![image](https://github.com/user-attachments/assets/6d8aa9c1-8419-4d6f-bf1f-ac994b087783) 
![image](https://github.com/user-attachments/assets/e3f2a9f9-9fa2-45e6-9d73-d1782b6fab85)























Next thing we will put in our dashboad is the 'user prompt'. To do that, click 'Tools' then select 'Page'. drag it to the dashboard.

![image](https://github.com/user-attachments/assets/c572e63e-998d-419c-a84f-9b615ab43ecf)


rename as 'User Prompt'. In the description, put 'Isolate Computer (Yes/No)'. you can also put a success message.
![image](https://github.com/user-attachments/assets/4b9b71df-1547-4242-a8c4-8393e4c3389e)


connect it to the 'Webhook' then click 'Edit page' at the bottom of build section.
![image](https://github.com/user-attachments/assets/94dee7a3-f507-4675-bb42-de6af93003d2)


Customize the fields and put a boolean button for Yes and No option.

![image](https://github.com/user-attachments/assets/d6464597-a337-4451-a18a-e8ec4364d480)





Let's edit the message that will be sent in slack and email. base on the diagram i created, the details to be sent is :


Time
Computer Name
Source IP
Process
Command Line
File Path
Sensor ID
Link to detection (if applicable)


