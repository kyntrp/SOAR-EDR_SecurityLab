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


  












