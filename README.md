# Windows Server 2022 & Active Directory Setup for Home Labs

The following setup guide is derived from East Charmer's [**Installing Active Directory on Windows Server on Virtual Machine (Home Lab)**](https://www.youtube.com/watch?v=GsmJowwIh8Q) tutorial

## Required:
* Windows Server 2022 ISO https://www.microsoft.com/en-us/evalcenter/download-windows-server-2022
* VMWare Workstation Pro https://knowledge.broadcom.com/external/article?articleNumber=368667

## Creating a Virtual Machine

After installing VMWare Workstation Pro and downloading the Windows Server 2022 ISO, we can begin with setting up a virtual machine that runs Windows Server 2022 by opening up VMWare Workstation Pro and clicking on "Create a New Virtual Machine"

<img width="379" height="163" alt="image" src="https://github.com/user-attachments/assets/d31f5709-758a-4c4c-a743-adfdf5a6834e" />

Create the virtual machine using these settings

<img width="317" height="315" alt="image" src="https://github.com/user-attachments/assets/71908483-f2dd-4834-a621-ae5a1fe685cb" />
<img width="313" height="313" alt="image" src="https://github.com/user-attachments/assets/6e53bbcf-8a81-421a-90d9-29be813e1065" />
<img width="320" height="316" alt="image" src="https://github.com/user-attachments/assets/c3ca388d-434f-4e67-b9de-7345747c3883" />

In the window that specifies disk capacity, just the recommended 60.0 GB and the default "Split virtual disk into multiple files" should be fine. Afterwards, we should now have created our blank Windows Server 2022 virtual machine

Right-click on the newly created virtual machine and click "Settings". Under "CD/DVD (SATA)" we want to check the box "Connected at power on", set "Connection" to "Use ISO image file:", and browse for the Windows Server 2022 ISO file (SERVER_EVAL_x64FRE-en-us.iso)

<img width="749" height="325" alt="image" src="https://github.com/user-attachments/assets/3f098689-8f9a-4346-818c-ca0c4c497b95" />

## Installing Windows Server 2022 onto the Virtual Machine

Now we can click on "Power on this virtual machine"

Upon seeing the "Press any key to boot from CD or DVD...", quickly press any key to load the ISO

**If the "EFI Network..." text shows up, press CTRL+ALT to return control back to your desktop, right-click on the virtual machine, hit "Shut Down Guest", and try again (verify that the "CD/DVD (SATA)" configuration is correct**

Install the Windows Server 2022 using "Windows Server 2022 Standard Evaluation (Desktop Experience)" and "Custom: Install Microsoft Server Operating System only (advanced)"

<img width="545" height="385" alt="image" src="https://github.com/user-attachments/assets/6bcf0bbf-fd82-42f1-9861-cbcde209cee6" />
<img width="506" height="279" alt="image" src="https://github.com/user-attachments/assets/8e5855ac-0417-4963-881a-0abf5384704e" />

Once installed, the virtual machine should reboot and then present a password setup for the Administrator account (make sure it's something memorable)

After setting up a password, login to the Administrator account using the password

From this point and onwards the Windows Server 2022 180-days free trial kicks in, so once it expires, we can build and install Windows Server 2022 onto a new virtual machine again from scratch

Now, we need to install Active Directory tools

## Install Active Directory Tools

Upon logging in as Administrator, we are presented with "Server Manager" in our taskbar

<img width="1023" height="724" alt="image" src="https://github.com/user-attachments/assets/51ada4d9-a81b-48c7-8660-8134d8827d6e" />

At the top-right of the Server Manager window, click on "Manage" > "Add Roles and Feature"

<img width="321" height="172" alt="image" src="https://github.com/user-attachments/assets/0e9822b6-f772-4c42-a59b-069323de5b96" />

Continue with these settings

<img width="614" height="435" alt="image" src="https://github.com/user-attachments/assets/b8e5a40c-8b62-4988-9ccc-d7d6719ec271" />
<img width="615" height="435" alt="image" src="https://github.com/user-attachments/assets/92a928e3-bf02-4ba8-8291-e58978fc3172" />

In the "Server Roles" section, check the box for "Active Directory Domain Services", "Remote Access", and "DNS Server"

<img width="614" height="437" alt="image" src="https://github.com/user-attachments/assets/0e71b8ff-0fcf-484d-9883-92d64f4a65de" />

In the "Features" section, ensure that the checkbox for "Group Policy Management" is checked (should be on by default from "Active Directory Domain Services")

In the "Remote Access" > "Role Services" section, check the box for "DirectAccess and VPN (RAS)"

<img width="779" height="554" alt="image" src="https://github.com/user-attachments/assets/3790bd25-fc0b-40ca-a8b9-b2541ac7e404" />

Proceed with clicking "Next" on each window, and hit "Install" at the final step at "Results"

**Once the installation is done, click on "Promote this server to a domain controller"**

In the "Deployment Configuration" section, select "Add a new forest", and then specify a name to be as the root domain name (adding a '.local' at the end of the root domain name)

<img width="631" height="432" alt="image" src="https://github.com/user-attachments/assets/7d944787-0efe-4aa3-9525-b735fd0e7c2f" />

In the "Domain Controller Options" section, ensure that both "Forest functional level:" and "Domain functional level:" are set to the latest Windows Server version (at time of writing is Windows Server 2016)

Set a password in the "Directory Services Restore Mode (DSRM)" fields

<img width="623" height="455" alt="image" src="https://github.com/user-attachments/assets/a5eda230-7222-462e-9345-9feca10260d9" />

Proceed with clicking "Next" on each window, and hit "Install" at the "Prerequisites Check" section (it should take a few minutes)

Afterwards, the virtual machine should automatically reboot

Upon logging in as Administrator, we should now see that we are logging in as "-root domain name-/Administrator"

To verify our installation, we should see the "Windows Administrative Tools" folder in the Windows start menu

## Basic Active Directory Setup

