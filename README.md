# Windows Server 2022 & Active Directory Setup for Home Labs

The following setup guide is derived from East Charmer's [**Installing Active Directory on Windows Server on Virtual Machine (Home Lab)**](https://www.youtube.com/watch?v=GsmJowwIh8Q) tutorial

## Required:
* Windows Server 2022 ISO https://www.microsoft.com/en-us/evalcenter/download-windows-server-2022
* VMWare Workstation Pro https://knowledge.broadcom.com/external/article?articleNumber=368667

# Creating a Virtual Machine

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

# Install Active Directory Tools

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

# Basic Active Directory Setup

Terms to know
* Forest
  * What it is
    * The entire "Tree" (e.g., all of .local)
  * Relation to group scopes
    * The ultimate limit for Universal groups
* Domain
  * What it is
    * The partition (e.g., sales.corp.local)
  * Relation to group scopes
    * The primary boundary for Domain Local/Global scopes
* Organizational Unit (OU)
  * What it is
    * A folder inside the domain
  * Relation to group scopes
    * Irrelevant to how scopes function

Open "Active Directory Users and Computers"

On the left column, we should see the created .local directory which we can expand to see the subdirectories it comes with

We want to create OUs for different departments

Right-click on the domain name > New > Organizational Unit

<img width="587" height="463" alt="image" src="https://github.com/user-attachments/assets/ade3f8d8-a58e-4221-8a39-187f752256a6" />

Create 3 OUs under the name "Canada", "Europe", and "Asia"

<img width="193" height="184" alt="image" src="https://github.com/user-attachments/assets/25b8be00-4d52-487b-955e-6ee2e5712670" />

Next, we want to create user accounts and groups within these OUs by nesting OUs

Inside each of the geographical OUs created, create 3 OUs under the name "Computers", "Users", and "Servers"

<img width="226" height="383" alt="image" src="https://github.com/user-attachments/assets/c432a604-1f35-4ae7-a5b6-aaaea89567ba" />

Next, we want to create different groups under the nested OUs to mimick a typical workplace environment (e.g., different company departments under "Users")

Right-click on "Users" under "Canada" > New > Group

<img width="465" height="223" alt="image" src="https://github.com/user-attachments/assets/eb16cba7-638d-41ca-b848-ddeef2d9528a" />

Before proceeding, we can familiarize ourselves with the different options this window presents

**Group scope**
* Domain local
  * Membership: anywhere in the forest
  * Permissions: within the same local domain (the .local name)
* Global
  * Membership: only the local domain (the .local name)
  * Permissions: anywhere in the forest
* Universal
  * Membership: anywhere in the forest
  * Permissions: anywhere in the forest

**Group type**
* Security: used to assign permissions to shared resources
  * Assign user rights
    * Built-in security groups
      * Domain admins for IT staff
      * Remote Desktop Users: users who needs remote access
    * Custom security groups
      * Finance department: access financial applications and data
      * HR department
  * Assign permissions (for a shared resource): determines who can access certain resources and level of access (full control, read, modify)
    * File and folder permissions
    * Printer permissions
    * Shared network permissions
* Distribution: used to create email distribution lists to send email to collections of users by using an email application like Exchange Server
  * All employees
  * Department-based (finance, IT, HR)
  * Role-based (executives, managers, admins)

Set the "Group name:" to "IT"

Leave the "Group type" to "Security" because it's going to provide user rights only to the "IT" department

Create another group under "Canada" > "Users" and name it "DL-ITAdmins" (short for "distribution list for IT admins")

Set the "Group type" to "Distribution" because we want this to be an email list for all IT users

Now we can start creating different users

Right-click on "Users" under "Canada" > New > User

Fill out the form as follows

<img width="434" height="376" alt="image" src="https://github.com/user-attachments/assets/205153f2-043b-4189-b18f-575daf208768" />
<img width="435" height="376" alt="image" src="https://github.com/user-attachments/assets/8405e93e-3cfd-4478-a0f9-cc5f562bdfb2" />

**Optional: create the groups "Accounting", "HR", "Sales", and "Management", all under the "Security" group type**

<img width="525" height="302" alt="image" src="https://github.com/user-attachments/assets/07506394-614b-421a-891d-1b414bcea6b9" />

Repeat the entire process for the "Europe" and "Asia" OUs (creating the groups and a user)

# Creating and Setting up GPOs

Group Policy Object (GPO) is the term for the collection of policies in Active Directory that can be applied to the domains and OUs. GPOs are typically used by admins to manage settings that are applied to users and computers

Open "Group Policy Management"

Inside this application, we can see the domains and OUs we have created inside Active Directory

<img width="750" height="527" alt="image" src="https://github.com/user-attachments/assets/b9b7645c-6a8f-4046-9198-132991d9d014" />

To see what GPOs are like, expand the "Group Policy Objects" folder, right-click "Default Domain Controller Policy" > "Edit..."

This opens up the editor for GPOs where we can create different policies for the domain

<img width="782" height="560" alt="image" src="https://github.com/user-attachments/assets/0a4d9ae6-eae3-4bbf-8207-486a1f3bfd1e" />

To know what GPOs we want to create, we need to familiarize ourselves with the differences between "Computer Configuration" vs. "User Configuration", and "Policies" vs. "Preferences"

**Computer Configuration**
* Settings in this node apply to the local computer, regardless of who is logged in. These are system-wide changes that affect the hardware, security, and OS behavior
* Policies are applied as soon as you see the Windows logo, before the user logs in

**User Configuration**
* Settings in this node follow the user. Whether they log into a desktop, a laptop, or a terminal server, their specific environment will "roam" with them
* Policies are applied as soon as the user enters their password and hits "Enter"

**Policies**
* Can't be changed by users
* Password policies, account lockout policies

**Preferences**
* Can be changed by users
* Mapped network drives, printers, desktop shortcuts


## Activity 1: Set a password policy to enforce strong passwords and enhance security

Right-click on the .local domain name > "Create a GPO in this domain, and Link it here..."

Name this GPO policy "Password Policy", and click "Ok"

Right-click on "Password Policy" > "Edit..."

This is where we can set up and configure our password policies

This policy should be under "Computer Configuration" because it is the computer that defines the security standard for the domain or machine

This policy should also be under "Policies" because it should never be altered by the user

We can find the password policy under "Computer Configuration" > "Policies" > "Windows Settings" > "Security Settings" > "Account Policies" > "Password Policy"

<img width="783" height="561" alt="image" src="https://github.com/user-attachments/assets/d944e3d4-7571-47db-9815-6911f0a29d85" />

From here, we can see all the different settings we can change for the GPO

To enable a policy, double-click on a policy, check the "Define this policy setting" box, fill out the form, hit "Apply" and "Ok"


## Activity 2: Map network drives for users when they log in

To explain this policy, what we are creating is a pointer to a folder inside the server, which users can access that pointer as a network drive. Why this is created as a network drive is to avoid long nested directories like "\A\B\C\D\E\folder", when users can just access that pointer as "E:\folder"

Right-click on the .local domain name > "Create a GPO in this domain, and Link it here..."

Name this GPO policy "Drive Mapping", and click "Ok"

Right-click on "Drive Mapping" > "Edit..."

This policy should be under "User Configuration" because we need this policy to follow the user on whichever device they use to log in

This policy should also be under "Preferences" because the user can add drive maps if they choose to

We can find the drive maps policy under "User Configuration" > "Preferences" > "Windows Settings" > "Drive Maps"

<img width="783" height="561" alt="image" src="https://github.com/user-attachments/assets/656dbcf6-0827-4102-9366-31a1784f39e6" />

Right-click "Drive Maps" > "New" > "Mapped Drive"

In this window, we can set a folder that users under the GPO can access as a drive label

<img width="397" height="451" alt="image" src="https://github.com/user-attachments/assets/220977d5-9f56-454d-96af-099ac5e775a1" />

After setting the location and drive letter, hit "Apply" and "Ok"


## Activity 3: Set a default desktop wallpaper for all users

Right-click on the .local domain name > "Create a GPO in this domain, and Link it here..."

Name this GPO policy "Desktop Wallpaper", and click "Ok"

Right-click on "Desktop Wallpaper" > "Edit..."

This policy should be under "User Configuration" because we need this policy to follow the user on whichever device they use to log in

This policy should be under "Policies" because we do not want to user to change their desktop wallpaper

We can find the desktop wallpaper policy under "User Configuration" > "Policies" > "Administrative Templates: ..." > "Desktop" > "Desktop"

<img width="784" height="561" alt="image" src="https://github.com/user-attachments/assets/8c65cbf9-c812-4196-ab04-faa5bbecfa86" />

Double-click "Desktop Wallpaper", and enable the policy at the left-side of the window

To set a custom wallpaper, set the wallpaper path and style underneath the "Options:" section, and then hit "Apply" and "Ok"


## Activity 4: Prevent users from accessing the Control Panel

Right-click on the .local domain name > "Create a GPO in this domain, and Link it here..."

Name this GPO policy "Restrict Control Panel", and click "Ok"

Right-click on "Restrict Control Panel" > "Edit..."

This policy should be under "User Configuration" because we need this policy to follow the user on whichever device they use to log in

This policy should be under "Policies" because it should be enforced that the Control Panel is restricted, no changes can be made

We can find the Control Panel policies under "User Configuration" > "Policies" > "Administrative Templates: ..." > "Control Panel"

<img width="783" height="561" alt="image" src="https://github.com/user-attachments/assets/a413327a-6893-438b-a984-fe8fba2d3354" />

Double-click "Prohibit access to Control Panel and PC settings", enable the policy, and then hit "Apply" and "Ok"

<img width="682" height="632" alt="image" src="https://github.com/user-attachments/assets/1af43306-3784-4b40-9f70-573492594266" />


## Activity 5: Prevent users from using USB storage devices

Right-click on the .local domain name > "Create a GPO in this domain, and Link it here..."

Name this GPO policy "Disable USB Devices", and click "Ok"

Right-click on "Disable USB Devices" > "Edit..."

This policy should be under "Computer Configuration" because we want the USB ports to be locked down the moment the computer boots up, eliminating any kind of bypassing

This policy should be under "Policies" because it should be enforced that the USB devices are restricted, no changes can be made

We can find the USB storage device policies under "Computer Configuration" > "Policies" > "Administrative Templates: ..." > "System" > "Removable Storage Access"

<img width="783" height="561" alt="image" src="https://github.com/user-attachments/assets/00a864c3-176a-4eae-a795-c65e7d00c15c" />

Double-click "All Removable Storage classes: Deny all access", enable the policy, and then hit "Apply" and "Ok"

<img width="682" height="633" alt="image" src="https://github.com/user-attachments/assets/cf37ab6a-a8cb-4f2b-82ea-451c2fb7f0db" />


## Activity 6: Configure account lockout settings to prevent brute-force attacks

Right-click on the .local domain name > "Create a GPO in this domain, and Link it here..."

Name this GPO policy "Account Lockout Policy", and click "Ok"

Right-click on "Account Lockout Policy" > "Edit..."

This policy should be under "Computer Configuration" because we want the computer to lock down before the user logs in after a number of attempts

This policy should be under "Policies" because it should be enforced that the accounts are locked out after a certain number of attempts, no changes can be made

We can find the Account Lockout policy under "Computer Configuration" > "Policies" > "Windows Settings" > "Security Settings" > "Account Policies" > "Account Lockout Policy"

<img width="783" height="560" alt="image" src="https://github.com/user-attachments/assets/a770dc8b-e299-4fd2-ad61-40f4905a15cb" />

Double-click "Account lockout threshold", enable the policy, set a threshold, hit "Apply" and "Ok"


# Applying and Testing GPOs

## Required
* Windows 11 Enterprise ISO https://www.microsoft.com/en-us/evalcenter/download-windows-11-enterprise


### Creating a Windows 11 Enterprise VM

Create a new blank VM, with this time changing the operating system version to "Windows 11 x64"

<img width="422" height="421" alt="image" src="https://github.com/user-attachments/assets/9734f30e-ca7e-462b-bb1f-2d2b265f0062" />

In the window that specifies disk capacity, I'm using 32.0 GB and keep the default "Split virtual disk into multiple files". Afterwards, we should now have created our blank Windows 11 virtual machine

Right-click on the newly created virtual machine and click "Settings". Under "CD/DVD (SATA)" we want to check the box "Connected at power on", set "Connection" to "Use ISO image file:", and browse for the Windows 11 Enterprise ISO file (26200.6584.250915-1905.25h2_ge_release_svc_refresh_CLIENTENTERPRISEEVAL_OEMRET_x64FRE_en-us.iso)

<img width="749" height="303" alt="image" src="https://github.com/user-attachments/assets/8ccaf0f1-1299-4f2c-ac5c-1bef5c47b6a6" />

Install Windows 11 Enterprise, just like how we did for the Windows Server 2022

While Windows 11 Enterprise is installing, we want to set our server to use a static IP address

### Setting up a Server Static IP

On the Windows Server VM, right-click on the Windows logo at the bottom-left of the taskbar > "Windows PowerShell"

This opens up the terminal that we can use to find the current IP address to set as static

Run the command "ipconfig"

We should now see the IP configuration that we want to set as static

<img width="718" height="303" alt="image" src="https://github.com/user-attachments/assets/05eef2c2-a58b-46dc-80aa-143d47cac079" />

Keep this terminal open, and proceed with the next step

Open up "Settings" > "Network & Internet" > "Change adapter options" > Right-click on "Ethernet0" > "Properties" > "Internet Protocol Version 4 (TCP/IPv4)" > "Properties"

**If you are unable to access "Settings" because it closes every time it opens, unlink "Restrict Control Panel" policy from `-root domain name-.local` and run `gpupdate /force` in a terminal**

Fill in the fields using the corresponding IP addresses found in the terminal window, and hit "Ok"

<img width="394" height="450" alt="image" src="https://github.com/user-attachments/assets/4f18facd-6f01-4f15-9b47-813b75e04936" />

We have now set the current IP address as static, which we will later use as the DNS server IP on our client machines

In the Windows 11 Enterprise setup, if prompted for a Microsoft account login, click "Show more options" > "Domain join instead"

To add the Windows 11 Enterprise VM into the domain, we first need to set up the DNS to point to the server

On the Windows 11 Enterprise VM, open up "Settings" > "Network & Internet" > "Properties" > "DNS server assignment:" > "Edit"

Change the dropdown box from "Automatic (DHCP)" to "Manual", set the "Preferred DNS" to the server's static IPv4 address, and then hit "Save"

<img width="478" height="590" alt="image" src="https://github.com/user-attachments/assets/b901df5d-46bc-4a14-a172-20717106c2fa" />

To verify that we have connectivity with our domain controller, we can run `ping -static server IP-` in a terminal

<img width="540" height="240" alt="image" src="https://github.com/user-attachments/assets/b5ee8b04-63ef-45b4-8d44-86d880fb8b6e" />

To also test if the domain controller is working, we can run `nslookup -root domain name-.local` in a terminal, and it should resolve the DNS

<img width="330" height="123" alt="image" src="https://github.com/user-attachments/assets/3cac5a2d-16a6-4f49-9dcc-3aa077b7dedc" />


### Joining the Computer to the Domain

On the Windows 11 Enterprise VM, open "Settings" > "System" > "About" > "Related links" > "Domain or workgroup"

In this window, click the "Change..." button on "To rename a computer or change its domain or workgroup, click Change."

<img width="403" height="459" alt="image" src="https://github.com/user-attachments/assets/3243bdba-0e43-4e3c-8db6-2d6590904328" />

Change the name of the PC to something memorable and identifiable

Change the "Member of" option to "Domain:", set the text field to `-root domain name-.local`, and then hit "Ok"

<img width="319" height="382" alt="image" src="https://github.com/user-attachments/assets/a814b203-41bd-4d83-a38b-04d3ff606e84" />

If prompted to enter credentials, enter `administrator` as the user and `-server password-` as the password

Restart the Windows 11 Enterprise VM for the changes to apply

Upon signing in to a user account, click "Other user" and try logging in with a user account created in the server's active directory

Now that we have joined the Windows 11 Enterprise VM to the domain, we need to move the machine to its respective OU

On the Windows Server 2022 VM, open "Active Directory Users and Computers"

Inside "`-root domain name-.local`" > "Computers", we should see the name of the Windows 11 Enterprise PC

Right-click the PC name > "Move..." > "Canada" > "Computers"

<img width="314" height="323" alt="image" src="https://github.com/user-attachments/assets/2acf9c43-c5ae-40eb-8537-d435fd548fca" />

Hit "Ok" and the PC should be moved to the "`-root domain name-.local`" > "Canada" > "Computers" OU


### Applying GPOs

On the Windows Server 2022 VM, open "Group Policy Management"

Expand the "Group Policy Objects" folder, and we can see all the GPOs we have created

<img width="749" height="526" alt="image" src="https://github.com/user-attachments/assets/1db6e9ee-2216-4d97-902c-f459e916a6fe" />

To apply a GPO to an OU, simply drag-and-drop the GPOs that you want to apply from the "Group Policy Objects" folder onto the destination OU

<img width="749" height="525" alt="image" src="https://github.com/user-attachments/assets/825b44e9-fc31-4418-b739-5e66cab0d26b" />

However, the GPO does not apply immediately

We can change this by running `gpupdate /force` on the client machine that has the GPO applied to

<img width="473" height="165" alt="image" src="https://github.com/user-attachments/assets/78bf9caf-3a38-4365-88bf-4d8d893f1f5c" />


# Setting up Network Sharing on Windows Server


