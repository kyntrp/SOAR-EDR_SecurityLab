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


Once the download is complete. open the Windows Powershell in administrator. navigate to Downloads directory (`cd C:\Users\vboxuser\Downloads`)
then type `.\<insert filename of the downloaded file from LimaCharlie>`
