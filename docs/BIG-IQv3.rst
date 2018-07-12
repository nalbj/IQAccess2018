BIG IQ 5.4 Self Guided Lab Guide

Participant Hands-on Lab Guide

Version: 3.0

|image0|

    Last Updated: 7/12/2018

©2018 F5 Networks, Inc. All rights reserved. F5, F5 Networks, and the F5
logo are trademarks of F5 Networks, Inc. in the U.S. and in certain
other countries. Other F5 trademarks are identified at f5.com.

Any other products, services, or company names referenced herein may be
trademarks of their respective owners with no endorsement or
affiliation, express or implied, claimed by F5.

These training materials and documentation are F5 Confidential
Information and are subject to the F5 Networks Reseller Agreement. You
may not share these training materials and documentation with any third
party without the express written permission of F5.

`Overview 4 <#_Toc518815019>`__

`Introduction 4 <#_Toc518815020>`__

`Device Information 5 <#device-information>`__

`Dependencies 6 <#dependencies>`__

`How to access the lab in the Ravello environment 7 <#_Toc518815023>`__

`The BIG-IQ User Interface 7 <#_Toc518815024>`__

`Note: 10 <#note>`__

`BIG-IQ Access Policy Manager 11 <#big-iq-access-policy-manager>`__

`WORKFLOW 1: Access Policy Review
11 <#workflow-1-access-policy-review>`__

`WORKFLOW 2: Location Specific Object Modification
12 <#workflow-2-location-specific-object-modification>`__

`WORKFLOW 3: Modifying an existing APM access policy using VPE
14 <#workflow-3-modifying-an-existing-apm-access-policy-using-vpe>`__

`WORKFLOW 4: Create a new VPN Access profile
19 <#workflow-4-create-a-new-vpn-access-profile>`__

`WORKFLOW 5: View APM Audit logs and Dashboards 26 <#_Toc518815031>`__

`BIG-IQ Device Management 33 <#big-iq-device-management>`__

`WORKFLOW 1: Setting up of BIG-IQ Data Collection Devices (DCD).
(REQUIRED)
33 <#workflow-1-removing-previously-discovered-devices-for-this-lab-exercise>`__

`WORKFLOW 2: Importing BIG-IP devices for management and inventory
(REQUIRED) 36 <#_Toc518815034>`__

`WORKFLOW 5: Automating device backups and archiving a copy of the
backup file 44 <#_Toc518815035>`__

`WORKFLOW 6: Uploading QKviews to iHealth for a support case
47 <#_Toc518815036>`__

`BIG-IQ Partial Deployment \| Partial Restore 51 <#_Toc518815037>`__

`WORKFLOW 1: Create multiple changes. Deploy single change. (REQUIRED)
51 <#_Toc518815038>`__

`WORKFLOW 2: Create and deploy multiple changes with selected roll-back.
(REQUIRED) 60 <#_Toc518815039>`__

Overview
========

This document details the lab exercises and steps that should be
followed by the student to learn about BIG-IQ Access specific functions
as they relate to managing BIG-IP Access Policy Manager.

The environment is setup with basic configuration and associated traffic
generation to populate dashboards for these exercises. BIG-IQ could be
managing BIG-IPs in Azure and Google Cloud, as well as on premesis for
example. This can be a powerful management tool for customers that are
talking about multi-cloud management.

Introduction

This lab environment is designed to allow for quick and easy
demonstration of a significant portion of the BIG-IQ product. The Linux
box in the environment has multiple cron jobs that are generating
traffic that populates the monitoring tab.

|image1|

Device Information
==================

+------------------------------------------+-------------+--------------------+------------------+---------------------------------------------+
| Device Name                              | Mgmt IP     | Version            | Username/pw      | Provisioning                                |
+==========================================+=============+====================+==================+=============================================+
| BIGIQ\_CM\_5.4                           | 10.1.1.4    | 5.4.0              | admin/admin      | BIG-IQ Console                              |
|                                          |             |                    |                  |                                             |
|                                          |             |                    | root/default     |                                             |
+------------------------------------------+-------------+--------------------+------------------+---------------------------------------------+
| BIGIQ\_DCD\_5.4                          | 10.1.1.6    | 5.4.0              | admin/admin      | BIG-IQ Data Collection Device               |
|                                          |             |                    |                  |                                             |
|                                          |             |                    | root/default     |                                             |
+------------------------------------------+-------------+--------------------+------------------+---------------------------------------------+
| BOS-vBIGIP01.termmarc.com                | 10.1.1.10   | 13.1.0             | admin/admin      | LTM, DNS, ASM, AFM, APM                     |
|                                          |             |                    |                  |                                             |
|                                          |             |                    | root/default     |                                             |
+------------------------------------------+-------------+--------------------+------------------+---------------------------------------------+
| BOS-vBIGIP02.termmarc.com                | 10.1.1.11   | 13.1.0             | admin/admin      | LTM, DNS, ASM, AFM, APM                     |
|                                          |             |                    |                  |                                             |
|                                          |             |                    | root/default     |                                             |
+------------------------------------------+-------------+--------------------+------------------+---------------------------------------------+
| ip-10-1-1-7.us-west-2.compute.internal   | 10.1.1.7    | 12.1.1 HF1         | admin/admin      | LTM, DNS, AFM                               |
|                                          |             |                    |                  |                                             |
|                                          |             |                    | root/default     |                                             |
+------------------------------------------+-------------+--------------------+------------------+---------------------------------------------+
| ip-10-1-1-8.us-west-2.compute.internal   | 10.1.1.8    | 13.1.0             | admin/admin      | LTM, FPS, ASM, APM                          |
|                                          |             |                    |                  |                                             |
|                                          |             |                    | root/default     |                                             |
+------------------------------------------+-------------+--------------------+------------------+---------------------------------------------+
| ip-10-1-1-9.us-west-2.compute.internal   | 10.1.1.9    | 12.1.1 HF1         | admin/admin      | LTM, DNS, AFM                               |
|                                          |             |                    |                  |                                             |
|                                          |             |                    | root/default     |                                             |
+------------------------------------------+-------------+--------------------+------------------+---------------------------------------------+
| Lamp-Server                              | 10.1.1.5    | CentOS 7 (UDF)     | root/default     | App Server, LDAP, Radius, TACACS+, syslog   |
|                                          |             | Ubuntu (Ravello)   |                  |                                             |
|                                          |             |                    | centos/default   |                                             |
+------------------------------------------+-------------+--------------------+------------------+---------------------------------------------+

Dependencies
============

-  The BIG-IP device must be located in your network.

-  The BIG-IP device must be running a compatible software version.

-  Enable basic authentication on BIG-IQ using set-basic-auth on in the
   shell.

***BIG-IP Versions*** AskF5 SOL with this info:

https://support.f5.com/kb/en-us/solutions/public/14000/500/sol14592.html

**Note:** Port 22 and 443 must be open to the BIG-IQ management address,
or any alternative IP address used to add the BIG-IP device to the
BIG-IQ inventory.

How to access the lab in the Ravello environment
================================================

**Agility Lab specific instructions.**

For indivual access take the following steps:

**Ravello:**

Once the Instructor has started your Lab environment you will be
provided a URL/IP Address to access the environments Jump Host where you
will perform all the Lab Tasks.

The BIG-IQ User Interface
=========================

In this section, we will go through the main features of the user
interface. Feel free to log into the BIG-IQ device to explore some of
these features in the lab.

After you log into BIG-IQ, you will notice:

1) A navigation tab model at the top of the screen to display each high
   level functional area.

2) A tree based menu on the left-hand side of the screen to display
   low-level functional area for each tab.

3) A large object browsing and editing area on the right-hand side of
   the screen.

|image2|

-  Let us look a little deeper at the different options available in bar
   at the top of the page.

|image3|

-  At the top, each tab describes a high-level functional area for
   BIG-IQ central management:

-  Monitoring –Visibility in dashboard format to monitor performance and
   isolate fault area.

-  Configuration – Provides configuration editors for each module area.

-  Deployment – Provides operational functions around deployment for
   each module area.

-  Devices – Lifecycle management around discovery, licensing and
   software install / upgrade.

-  System – Management and monitoring of BIG-IQ functionality.

-  Application – Visibility for all of the components of the
   application.

-  Overview of left hand navigation for each top-level functional
   area.\ |image4|

Note: 
======

This Course is not intended to train you on the general device
management functions of BIG-IQ but rather to train you on the features
available within the Access portion of BIG-IQ for managing BIG-IP Access
Policy Manager module on multiple BIG-IPs throughout your organization.
Therefore we will jump right into the BIG-IQ Access specific related
workflows. Toward the end of the lab we will discuss what steps an Admin
must take in order to discover and import a BIG-IP device running Access
Policy Manager so that it can be managed by BIG-IQ.

BIG-IQ Access Policy Manager
============================

Objective
^^^^^^^^^

BIG-IQ can create, modify, and delete APM access and per-request
policies.

WORKFLOW 1: Access Policy Review
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Navigate to Configuration Access Access Groups BostonAG Access Policies
Per-Session Policies TestAccessProfile

|image5|

The access policy will be displayed in a new screen as shown below.
Compare the Access policy in BIG IQ with the policy in BIG IP source
device and ensure that they are exactly same. Open the browser shortcut
for the BIG-IP01 in a new tab from Chrome.

|image6| |image7|

WORKFLOW 2: Location Specific Object Modification
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

-  Navigate to Configuration Access Access Groups BostonAG
   Authentication Active Directory Active Directory

    |image8|

    LSO or Location Specific Objects are objects within an access
    profile/policy that relate to more specific geographic areas
    normally and are not shared between all devices by default to
    prevent misconfigurations. As an example, AAA servers are located in
    all office/data centers globally however if an end user is accessing
    a policy on an APM in Europe we wouldn’t want their authentication
    requests to be sent over a WAN link to some Domain Controller in
    another country and cause a tremendous delay for that user.

-  Click the check box for the FrogPolicy-olympus-ad for the BIGIP02
   device.

-  Click the Mark Shared button and accept the warning

    |image9|

    This will move the object from the device specific location to the
    Shared resources location.

-  Click on the AAA object to edit the properties

-  Change the Timeout value from 15 to 1500

-  Click Save & Close

    |image10|

    BIG IQ provides the ability to transition LSO objects to Shared
    Objects and vice versa. When an LSO object is made Shared it will
    have the same configuration across all the BIG IPs after deployment.

WORKFLOW 3: Modifying an existing APM access policy using VPE
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

-  Navigate to ConfigurationAccessAccess Groups

-  Select BostonAG

|image11|

Click on Access Policies -> Per Session Policies:

Select TestAccessProfile and add the following objects:

-  Logon page (accept default settings)

-  AD Auth using FrogPolicy-Olympus-AD

-  If AD Auth successful, your allowed access

|image12|

Start by hovering the mouse over the blue line in the policy flow
between the Start and Ending points and clicking the Green Plus sign.

|image13|

Now select the “Logon Page” object on the right side of the pop up
window. Then click “Save” on the next pop up window.

|image14|

The result should look like the picture below.

|image15|

Now repeat the steps by hovering the mouse on the blue line between the
Logon Page object and the Ending Deny and click the Green plus sign to
add the Authentication object of AD Auth.

|image16|

Now click the Server drop down to select FrogPolicy-olympus-ad and then
click “Save”.

|image17|

Change the Ending DENY to ALLOW.

Notice the Yellow Banner warning that there are un-saved changes. Click
the Save button at the bottom of the profile page. Click OK on the
Policy Save Conformation pop up window.

|image18|

After modifying the access profile, go to “Deployment tab- > Evaluate &
Deploy -> Access”

Click on Create in Evaluation section. Enter a name in the Name Field
then click the Checkbox in the Available section of Target Devices and
Click the arrow to the right to move both BOS BIGIP deivces to the
Selected area and then click the Create button at the bottom.

|image19|

The BIG-IQ will now start evaluating the configurations on the BIG-IP
devices and provide a comparison of the changes between the stored
configuration within the BIG-IQ versus the current running
configurations on the BIG-IP systems. When the evaluation completes you
will see a screen like the one below. Click the “VIEW” link under the
Access column.

|image20|

In the evaluation section, you will be able to view the added/changed
items. After reviewing click the Cancel button at the bottom of the pop
up window.

|image21|

Now click the Deploy button in the Evaluations section and wait for the
Deployment tast to complete.

|image22|

You can verify on BigIP that the access profile changes were pushed:

|image23|

WORKFLOW 4: Create a new VPN Access profile
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

-  Navigate to ConfigurationAccessAccess Groups

-  Select BostonAG

|image24|

You can see all of the access policies listed in the Per Session
Policies:

|image25|

Click Create and you will see the Access Policy creation screen. Give it
a name of “VPN-AP” and click on **Save & Close**. You can change the
view from Basic -> Advanced if you want to modify additional settings
such as timeouts, SSO, logout URI, etc..

|image26|

Then click “New” in macros and select “AD Auth and resources” template.
Then click the “OK” button.

|image27|

Click on the AD Auth object and use the Server drop down to select
FrogPolicy-olympus-ad then click Save.

|image28| |image29|

Now click the Resource Assign object. In the pop up window click the Add
button. Expand the Network Access section and move the
/Common/FrogPolicy-F5\_VPN from the Available section to the Selected
section and click the Save button.

|image30|

The result will look like the picture below, click the Save button on
this screen.

|image31|

Then add the macro into the VPE by hovering mouse over blue line and
selecting the Green plus sign. Then change the ending on the
“Successful” branch to **Allow**. Then click Save buttons to complete.

|image32|\ |image33|

|image34|

After creating and saving the access profile, go to “Deployment - >
Evaluate & Deploy -> Access”.

Click on “Create” in Evaluations, give it a name, and select
BOS-vBIGIP01/02 devices.

|image35|

Click on View after the evaluation is done to view the changes in Green.

|image36|

|image37|

Then Click on Deploy and verify the new VPN Access Profile is pushed
onto the BIG-IP device BOS01.

|image38|

|image39|

Objective
^^^^^^^^^

In this workflow the Student will learn how to navigate through and use
the BIG-IQ Centralized Management Access Monitoring tools to understand
how they can benefit an Administrators day to day Access tasks and also
how it can help with troubleshooting Access related issues.

WORKFLOW 5: View APM Audit logs and Dashboards 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Navigate to Monitoring Audit Logs Access

|image40|

Note: In case you do not have any data in BIG-IQ, check the active
session in Access tab in BIG-IP Boston Active cluster. If the session
shows pending (blue), restart the apmd process on the BIG-IP (bigstart
restart apmd).

We will now walk through several different Dashboards available under
the Access portion of BIG-IQ. During this exercise we will bring
attention to several key areas of interest for Adminstrators.

Start by following along the separate menu paths below to each sub-menu
section for Access Dashboards:

Navigate to Monitoring Dashboards Access

-  View Access Summary

   |image41|

   Notice the layout provides a great overview of usage of the entire
   Access infrastructure of devices which are currently under management
   with BIG-IQ. This single page view provide a quick snapshot view of
   license usage, Geographic access usage, top users, Session counts and
   Denied Sign-Ins. There is a time slider at the top of the page
   allowing the Admin to apply constraints of the time period for which
   the graphs and session counts should display. Take notice of the
   current Session counts and Sign-In Denied count, then adjust the left
   time slider moving it to the right slightly. Then adjust the right
   slider moving it to the left slightly. You will notice the session
   counts have changed. Now notice from this point an Admin could
   quickly drill down into certain areas of interest for
   troubleshooting. Click on the Sign-in Denied number to review further
   details. On the lower portion of this page you will find a list of
   denied sessions. You can see the duration of the session for the
   given user along with the username, client ip, and in this example
   IP-Reputation matched that prevented access for many of the sessions.

-  Application Summary

   |image42|

   On the Application Summary screen we can see useage request for Top
   10 apps along with Bytes In/Out details and number of Unique Users
   per application. By clicking on an application name like Confluence
   we can drill down to the details for that specific application.

-  Federation -> SAML ->SP -> SP Summary

   |image43|

   Federation is being used more widely these days. The BIG-IP Access
   Policy Manager can perform both SAML Service Provider as well as
   Identity Provider functions. In this summary screen we see the
   Federated Assertions for foreign Identity Providers for Services
   (Applications) hosted from the Access Policy Managers in the
   organization.

   Once again an Admin can use this screen to start diagnosing issues
   like Failed Assertions by clicking the lines in that section for
   drill down details.

-  Federation -> SAML ->IdP -> IdP Summary

   |image44|

   In the IdP Summary screen we see when the BIG-IP Access Policy
   Manager is acting as the Identity Provider and providing assertions
   to external Service Provider hosted applications. Same drill
   down/troubleshooting benefits can be found here for the
   Administrators of the Access environment.

-  Remote Access -> Network Access -> Network Access Summary

   |image45|

   In the Network Access Summary screen you will notice something new
   between the user counts number at top and the graph below them. There
   are three TABS, Sessions, Connections, Bytes Transferred. You will
   currently be selected/presented with the Sessions Tab information.
   Click the Connections tab and review. Now click the Bytes Transferred
   tab. As of version 13.1 TMOS code that runs on the BIG-IPs the BIG-IQ
   5.4 can display these details for reporting and troubleshooting and
   capacity usage and planning.

-  Remote Access -> Network Access -> Network Access Usage

   |image46|

   This screen again is providing more detailed reporting of the Bytes
   In/Out/Transferred by given users for the Admin to utilize.

-  Remote Access -> VDI Summary

   |image47|

   Many companies have implemented the use of Virtual Desktop
   Infrastructures of the years for deploying either individual
   published applications or full desktops for users. This summary
   screen provides reporting on the usage of those VDI objects being
   served through the BIG-IP Access Policy Manager working as a VDI
   Proxy for the three major flavors of VDI technology from Microsoft
   RDP, VMWare Horizon and Citrix XenApp/XenDesktop.

-  Sessions -> Sessions Summary

   |image48|

   As we review the Session Summary screen you should notice under the
   ACTIVE column there are Green Dots for sessions that are currently
   active however this screen is displaying the list of all sessions
   even those denied sessions we reviewed earlier. You can click on the
   session ID to review the policy events for a given session.

-  Sessions -> Active

   |image49|

   In this screen we are only reporting the Currently Active Sessions.
   Notice the check box to the left of eash session. You can click to
   check a box and the button above “Kill Selected Sessions” will be
   un-grayed allowing the Admin to kill the checked sessions. If the
   Admin were to click the check box in the Column header it would check
   all sessions boxes and the Kill All Sessions and/or Kill Selected
   Sessions buttons would then perform the kill on all sessions. In both
   scenarios the Admin is presented with a Confirmation Screen before
   actually killing those checked sessions.

-  Sessions -> Bad IP Reputation

   |image50|

   In this section we can see the reported IP Reputation data for
   incoming requests to the APM Policies.

-  Sessions -> Bowsers and OS

   |image51|

   This screen provide details of browser types and OSes being used to
   access the APM policies. This is great information if an organization
   has specific policies in place that stipulate which Browsers and OSes
   that support. The Admin can quickly see where they fall in line with
   those policies.

-  Sessions -> By Geolocation

   |image52|

   This reporting screen provides a Heatmap displaying from where access
   attempts are being initiated from. If an organization only allowed or
   supported access from certain geographic regions this screen can
   provide quick details on possible bad actor attempts to the
   organizations Access infrastructure.

-  Endpoint Software -> Endpoint Software Summary

   |image53|

   You may need to reset the Timeframe either by adjusting the sliders
   or using the Timeframe dropdown. This screen provides information of
   Endpoint Software in use by clients and detected via the Endpoint
   Inspection helper applications that run on clients systems and report
   back to the BIG-IP Access Policy Manager during access.

-  Endpoint Software -> Endpoint Software Details

   |image54|

   This is another great troubleshooting screen to review versions of
   client AV software.

-  License Usage

   |image55|

   This screen provides an overview of the Access Policy Manager license
   usage for both Access Session licenses as well as Connectivity
   Session licenses per APM Device.

-  User Summary

   |image56|

   In the user summary screen one item that can be useful to an Admin is
   the Filter Search field by Username. If your organization has a large
   community of users accessing in many different methods or
   applications the ability to filter by username and drill into those
   sessions for a specific user are helpful for troubleshooting issues.

These were just a few of the screens available however taking the time
to review this Monitoring Dashboards with live data can be helpful in
getting familiar with Admin duties for Access Policy infrastructure
using the BIG-IQ Centralized Manager.

BIG-IQ Device Management
========================

The following workflows will get you familiar with the BIG-IQ for
management of BIG-IP devices specific to managing Access Policy Manager.
This course is not intented to walk through all the required steps to
implement BIG-IQ and setup logging and statistics collection. That
information can be referenced from the BIG-IQ CM Implementation Guide.
For this BIG-IQ Access lab we will proceed to general ADC management
tasks (importing a BIG-IP and managing its configuration).

WORKFLOW 1: Removing Previously Discovered devices for this LAB Exercise 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Since this Self Guided Lab started with the BIG-IQ pre populated with
the BIG-IP devices for the previous exercises we must now remove some of
those devices before continuing on to the next workflow.

Navigate to the top menu Devices tab then on the left menu select BIG-IP
Devices

Place a check in the box next to BOS-vBIGIP01.termmarc.com

|image57|

Now click the Remove All Services button.

|image58|\ |image59|

You will see on the services column the services are being removed. When
it displays only Management you can perform the same steps to the second
BIG-IP device named BOS-vBIGIP02.termmarc.com

Navigate to the Devices Tab BIG-IP Clusters menu Access Groups sub-menu

Verify if the BostonAG Access Group still exists and if so do the
following task otherwise skip to next step.

a. Place a check in the box next to BOSTON and click the REMOVE button

    |image60|

Navigate to BIG-IP Clusters DSC Groups

If there are any datasync groups from either of the BOS devices shown
then click the Refresh List button and verify they are removed.

|image61|

Navigate to Devices Device Groups menu

Verify the BostonDCGroup is gone, if not place a check in the box next
to BostonDCGroup and click the Delete button.

|image62|

Navigate to Devices BIG-IP Devices

Place a check in the box next to both BOS-vBIGIP01.termmarc.com and
BOS-vBIGIP02.termmarc.com BIG-IP devices and click the Remove Devices
button and confirm.

|image63|

|image64|

The Final result should only display the west BIG-IPs like the picture
below.

|image65|

Now logon to both the BOS-vBIGIP01.termmarc.com and
BOS-vBIGIP02.termmarc.com BIG-IP devices directly and verify they are no
longer showing that they are managed by BIG-IQ.

|image66|\ |image67|

WORKFLOW 2: Importing BIG-IP devices for management and inventory (REQUIRED)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Normally these steps are preformed first however we wanted to focus
first on management and monitoring of the Access infrastructure in the
beginning of the LAB. From this point forward we will be reviewing some
of the first step of managing devices for onboarding of BIG-IP devices
and their services which they are running using the BIG-IQ is device
discovery. The basic discovery allows for device inventory, device
health monitoring, backup and restore of the managed device, integration
with F5’s iHealth service, software upgrade, and device template
deployment. As part of the discovery process, you can choose to manage
other parts of the BIG-IP configuration.

In this scenario, we will import a pair (clustered) of BIG-IP devices,
review the device information available in BIG-IQ, export our inventory
to a CSV file, and review that.

    Adding devices to BIG-IQ Inventory:

***Dependencies: ***

1. The BIG-IP device must be located in your network.

2. The BIG-IP device must be running a compatible software version.

***BIG-IP Versions***

+------------------------------+------------------------------+
| **Functional Description**   | **Minimum BIG-IP version**   |
+==============================+==============================+
| Backup/Restore               | 11.5.0 HF7                   |
+------------------------------+------------------------------+
| Upgrade - legacy devices     | 10.2.0                       |
+------------------------------+------------------------------+
| Upgrade - managed devices    | 11.5.0 HF7                   |
+------------------------------+------------------------------+
| Licensing BIG-IP VE          | 11.5.0 HF7                   |
+------------------------------+------------------------------+
| Licensing - WebSafe          | 12.0.0                       |
+------------------------------+------------------------------+
| ADC management               | 11.5.1 HF4                   |
+------------------------------+------------------------------+
| AFM                          | 11.5.2                       |
+------------------------------+------------------------------+
| Access                       | 12.1.0                       |
+------------------------------+------------------------------+
| ASM                          | 11.5.3 HF1                   |
+------------------------------+------------------------------+
| DNS                          | 12.0.0                       |
+------------------------------+------------------------------+

 

AskF5 SOL with this info:
https://support.f5.com/kb/en-us/solutions/public/14000/500/sol14592.html

1. Port 22 and 443 must be open to the BIG-IQ management address, or any
   alternative IP address used to add the BIG-IP device to the BIG-IQ
   inventory.

Big-IP Devices
^^^^^^^^^^^^^^

Adding a BIG-IP device to the BIG-IQ system inventory is the first step
to management. First, we will be adding an HA pair of devices to be
managed in BIG-IQ.

**\*\*Important-** Before you attempt to add the BIG-IP cluster
(***BOS-vBIGIP01.termmarc.com*** and ***BOS-vBIGIP02.termmarc.com***),
make sure that the devices are **‘In Sync’** from a configuration
standpoint or you will get an error when attempting to import. You will
need to access one of the devices directly to do this. Log in to either
**BOS-BIGIP** from the UDF Components page and sync the configs\ **.**

\*\*DO NOT SKIP THE ABOVE STEP\*\*

1. Log in to the BIG-IQ system with your user name (admin) and password
   (admin).

2. On the top menu bar, select **Devices** from the BIG-IQ menu.

3. On the left-hand menu bar, click **BIG-IP Devices**.

4. Click the **Add Device** button in the main pane.

   a. In the **IP Address (10.1.1.10)** field, type the IPv4 or IPv6
      address of the device.

   b. In the **User Name** and **Password** fields, type the user name
      (admin) and password (admin) for the device.

   c. Cluster Display Name: Select **Create New.**

   d. Name the cluster **BostonCluster**

   e. Leave the **‘Initiate..’** radio button checked

|image68|

1. Click the Add button to add this device to BIG-IQ.

2. BIG-IQ now exchanges certs with the BIG-IP and pops up a window for
   the administrator to select which modules to manage from BIG-IQ. For
   this device, select all services except **Fraud Protection
   Services.** Leave the Statistics monitoring boxes all checked, and
   then click the **Continue** button.

    |image69|

1. The discovery process will start and you should see a screen like
   this. At this point, BIG-IQ is using REST calls to the BIG-IP to pull
   the selected parts of the BIG-IP configuration into BIG-IQ.

|image70|

While the discovery process is happening for the first device, add the
second device to BIG-IQ:

1. Click the **Add Device** button.

   a. In the **IP Address (10.1.1.11)** field, type the IPv4 or IPv6
      address of the device.

   b. In the **User Name** and **Password** fields, type the user name
      (admin) and password (admin) for the device.

   c. Cluster Display Name: Select **Use Existing.**

   d. Select **BostonCluster** from the list of existing clusters.

   e. Leave the **‘Initiate..’** radio button checked

2. Click the Add button to add this device to BIG-IQ.

3. For this device, again, select all services except **Fraud Protection
   Services.** Leave the Statistics monitoring boxes all checked, and
   then click the **Continue** button.

Allow the import jobs to complete. At this point, the configuration of
the BIG-IPs that have been imported are not yet editable in BIG-IQ. To
make the configurations editable in BIG-IQ, we need to |image71|.

1. On the Device Inventory screen, click the |image72|\ link in the
   Services column for **BOS-vBIGIP01**. *(you may need to scroll right
   to see the services column*)

|image73|

1. In the Local Traffic (LTM) Section, select the check box for “Create
   a snapshot of the current configuration before importing” and click
   the **Import** button.

|image74|

1. Before proceeding un-check the LTM snapshot box if still checked. In
   the Access Policy (APM) Section, select the check box for “Create a
   snapshot of the current configuration before importing” and click the
   **Import** button.

|image75|

BIG-IQ Access has its own notion of device grouping called “Access
Groups” where you define a “Source Device” where configuration changes
can be made and deployed to the other devices in the Access Group.
Create a new Access Group by choosing **Create** New from the Access
Group drop-down. Then name the new group **BostonAG**, and Click the Add
button to continue.

    |image76|

1. In the Application Security (ASM) Section, select the check box for
   “Create a snapshot of the current configuration before importing” and
   click the **Import** button.

|image77|

a. In step 14, you will experience “Conflict Resolution.” A conflict is
   when an object that is already in the BIG-IQ working config has the
   same name, but different contents as an object that exists on the
   BIG-IP that is being imported. The user must select whether to keep
   the object from BIGIP or BIGIQ configuration. Storage will be updated
   accordingly. Review the differences that have been discovered as part
   of this import by clicking on each row in the difference view.

    |image78|

a. In this lab, we are going to choose to keep the version of the object
   that is already in the BIG-IQ. Click the continue button.

b. A window reminds us that these conflict resolution selections will
   not modify the configuration that is running on this BIG-IP until we
   deploy changes from BIG-IQ. Click the Resolve button to continue.

    |image79|

1. In the Advanced Firewall (AFM) Section, select the check box for
   “Create a snapshot of the current configuration before importing” and
   click the **Import** button.

|image80|

a. Again, you will experience the conflict resolution screens. Choose to
   keep the objects that are already on the BIG-IQ.

1. In the BIG-IP (DNS) Section, click the **Import** button.

|image81|

1. Click the back arrow button at the top of the section to return to
   the inventory.

   |image82|

2. Repeat steps 11-20 for BOS-vBIGIP02

|image83|

-  For the APM import, make sure to choose **Add to existing** for the
   Access Group and select the **BostonAG.** Accept any conflicts.

1. Once you have completed all of the import tasks for **BIGIP02**,
   click the arrow in the upper left of the Services panel to return to
   the device inventory screen.

   |image84|

2. Click on the **BOS-vBIGIP01.termmarc.com** device link to review the
   device Properties, Health, and Services information for the device.

   |image85|

3. Click through the Properties, Health, Statistics Collection, and
   Services tabs to review the information.

4. Click the arrow in the upper left of the Services panel to return to
   the device inventory screen.

   |image86|

5. Repeat steps 20-22 for the other devices, if you wish.

6. Click the Export Inventory button in the main pane to review the
   contents of the device inventory CSV file

7. The CSV file is automatically downloaded to your client. Launch the
   CSV file from your downloads folder. For example, in Chrome the CSV
   file will appear in the lower left.

   |image87|

8. Review the contents of the file and understand all of the information
   that is provided. The picture below shows what the exported inventory
   would look like in Microsoft Excel. The JumpBox you are using for the
   LAB does not have Excel installed so you can choose to skip the
   review of the file or use Notepad.

   |image88|

WORKFLOW 5: Automating device backups and archiving a copy of the backup file 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Time to Complete: 5 min

BIG-IQ provides the ability to backup individual or groups of managed
devices on an ad-hoc or a scheduled basis. The admin can decide how long
to retain the backups on BIG-IQ and has the option of archiving a copy
of the UCS backup off to an external device for DR or deeper storage
purposes.

In this scenario, we are going to create a group of all of the devices
in our Boston data center and schedule a nightly backup that archives a
copy off to our archive for DR purposes.

First, we need to create the group for our backup schedule to reference.
We have two options in BIG-IQ: static groups, where devices are added
and removed manually and dynamic groups, where devices are selected from
a source group based on filter criteria. In this lab setup, the devices
have BOS in the name to indicate that they are in the Boston data
center. This makes the dynamic group the logical choice.

1. On the top menu bar, select **Devices** from the BIG-IQ menu.

2. Click **Device Groups** in the left-hand menu

3. Click **Create** in the main pane

4. | Complete the settings to create the group.
   | Name: **BostonDCGroup**
   | Group Type: **Dynamic**
   | Parent Group: **Root (All BIG-IP Devices)**
   | Search Filter: **BOS**

   |image89|

5. Click the **Save & Close** button to save the group.

Now, we can create our backup schedule that references this dynamic
group.

1. Click on the **Back Up & Restore** on the left-hand menu

2. Click on **Backup Schedules**

   |image90|

3. Click the **Create** button in the main page.

4. | Fill out the Backup Schedule
   | Name: **BostonNightly**
   | Local Retention Policy: **Delete local backup copy 3 days after
     creation**
   | Backup Frequency: **Daily**
   | Start Time 00:00 Eastern Standard Time

   Under Devices, select the **Groups** radio button

   | Select from the drop-down **BostonDCGroup**
   | Archive: **Store Archive Copy of Backup**
   | Location: **SCP**
   | IP Address: **10.1.10.80**
   | User name: f5
   | Password: default
   | Directory: /home/f5
   | |image91|

|image92|

1. Click **Save & Close** to save the scheduled backup job.

WORKFLOW 6: Uploading QKviews to iHealth for a support case
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

BIG-IQ can now push qkviews from managed devices to ihealth.f5.com and
provide a report of heuristic hits based on the qkview. These qkview
uploads can be performed ad-hoc or as part of a F5 support case. If a
support case is specified in the upload job, the qkview(s) will
automatically be associated/linked to the support case.

1. Navigate to **Monitoring** on the top menu bar and then to
   **REPORTS-> Device-> iHealth** -> **Configuration** on the left-hand
   menu\ **.**

   |image93|

2. Add Credentials to be used for the qkview upload and report
   retrieval. Click the Add button under Credentials.

   |image94|

3. | Fill in the credentials that you used to access
     https://ihealth.f5.com :
   | Name: **Give the credentials a name to be referenced in BIG-IQ**
   | Username: **<Username you use to access iHealth.f5.com>**
   | Password: **<Password you use to access iHealth.f5.com**>

4. Click the Test button to validate that your credentials work.

   |image95|

5. Click the **Save & Close** button in the lower right.

6. Click the **Tasks** button in the BIG-IQ iHealth menu.

   |image96|

7. Click the **QKView Upload** button to select which devices we need to
   upload QkViews from:

   |image97|

8. | Fill in the fields to upload the QkViews to iHealth.
   | Name: **QKViewUpload5346** (append the last 4 digits of your cell
     number to make this request unique)
   | Credentials: **<Select the credentials you just stored in step 5>**
   | Devices: Select **ip-10-1-1-7.us-west-2.compute.internal**

|image98|

1. Click the **Save & Close** button in the lower right. The task will
   be started immediately.

   \*Note that you can also schedule QKview uploads on a regular basis
   using the **QKView Upload Schedules** on the left menu bar

2. Click on the name of your upload job to get more details

   |image99|

3. Observe the progress of the Qkview creation, retrieval, upload,
   processing, and reporting. This operation can take some time, so you
   may want to move on to the next exercise and come back.

4. Once a job reaches the Finished status, click on the Reports menu to
   review the report.

   |image100|

5. Select the report you just created and click the **Open** hyperlink
   under the Report Column

   |image101|

6. You can also run the Upgrade Advisor from the BIG-IQ if you are
   running an older version of code. Select **Upgrade Advisor Reports**
   from the left-hand menu bar and then click the **Create** button in
   the main window pane.

7. Give your Upgrade Advisor Task a name and select the **ip-10-1-1-7**
   device. Choose your Target Version and then **Save & Close**

8. Click on the **Upgrade Advisor Reports** on the left-hand menu bar
   and your new report should show up shortly. You can see the status of
   the report generation by clicking **Tasks** on the left-hand
   menu-bar. Click on the **Target Software Version** column to view
   your results.

Now is a good time to circle back and see if any statistics have been
created for our BIG-IP inventory.

Navigate to the monitoring dashboards to validate that statistics are being collected and displayed for the BIG-IP devices.
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

-  Navigate to **MonitoringDashboards** **Device** **Health** to verify
   that the graphs are populated.

   |image102|

-  We are going to move on to other parts of the lab while we collect
   some stats and then we will circle back when we have more data to
   work with.

BIG-IQ Partial Deployment \| Partial Restore
============================================

WORKFLOW 1: Create multiple changes. Deploy single change. (REQUIRED)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Objective
^^^^^^^^^

The user has the ability to select a specific change out of many made
for deploy.

Partial Deployment
^^^^^^^^^^^^^^^^^^

**Method 1 –** Partial Deployment selection done from Deployment
EVALUATE & DEPLOY.

-  Add an additional node to pool member.

-  ConfigurationPools: Enter “app1pool” in the upper right Filter and
   search, Select pool “app1pool” on either BOS-vBIGIP01 or 02.

-  1\ :sup:`st` change

   -  Click on New Member

|image103|

-  Select from Existing Node “app1node1” on port 80 HTTP

    |image104|

-  Click on “Save and Close” on lower right

-  2\ :sup:`nd` change – Create a New Monitor “mon-https”

   -  Click on left navigation panel “Monitors” and click on
      “Create”\ |image105|

   -  New Monitor

      -  Name: mon-https

      -  Type: HTTPS

      -  Monitor template: https

      -  username: admin

      -  password: admin and confirm password.

   -  Click “Save and Close”

|image106|

-  Add newly created Health Monitor “mon-https” to Pool “app2pool”

   -  On Pools, search app2pool

   -  On Health Monitors, select /Common/mon-https

|image107|

-  Save and Close

-  Create evaluation and deploy changes

   -  Click on top Deployment tab

   -  On left navigation panel, under EVALUATE & DEPLOY: Local Traffic &
      Network

   -  Click Create under Evaluations

|image108|

-  Name: partial-deploy

-  From Evaluation: Source Scope, Select “Partial Changes”

-  From Source Objects: Available, select “Pools”, from pool list,
   select “app1pool” for

-  BOS-vBIGIP01 & 02, and add them to Selected on the right

-  Under Target Devices, click “Find relevant devices”, select both and
   add to right

-  Click “Create” to complete

|image109|

***Note:** Only changes to “app1pool” will be deployed.*

-  Deploy changes

   -  Two methods to deploy

      ***Note**: Method 2 will not be part of this deployment.*

-  Method 1

   -  Once Evaluation is completed, click on View link to see the
      differences

|image110|

-  Cancel to dismiss the popup window and click on Deploy under
   Evaluation

-  Confirm by click on Deploy

|image111|

**Method 2:** From “Configuration” tab on top, the user can select the
source object and deploy

-  Select both “app2pool” from Configuration: Pools, use filter if
   desired

-  Click “Deploy”

|image112|

-  ***Note**: This will add to the source objects list automatically for
   the evaluation task.*

|image113|

-  Partial Changes is selected and “app2pool” for both LTMs are added to
   source object list for page to create evaluation task.

-  You must use “Find relevant devices” to select the devices to move
   them to the right

-  After evaluation is finished, click on “View” to see the
   difference\ |image114|

-  |image115|

-  Click on “Deploy now” in the Schedule area to deploy

**Note:** The deployment could fail if the targeted BIG-IP devices are
not in full sync on configurations, due to timeout on waiting for sync
to complete on target devices. Ensure the devices are in full sync
before deploying changes.

WORKFLOW 2: Create and deploy multiple changes with selected roll-back. (REQUIRED)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Objective
^^^^^^^^^

Partial Restore – Roll Back a change
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Note: Use the two changes made in the step above.

-  Deployment: Evaluate & Deploy: Local Traffic & Network.

-  Create task using “Source Scope: All Changes”

-  Select Devices Targeted.

-  Verify all changes are part of the deployment.

   -  Add node to pool “app1pool”

   -  Add Health Monitor to “app2pool”

-  Deploy and observe completion

|image116|

-  To Rollback a change, you need to create a Partial Restore
   Evaluation.

   -  Deployment: RESTORE: Local Traffic & Network.

|image117|

-  Add name “partial-restore” and select the Snapshot created when
   deployment occurred.

   ***Note:** Duplicate names are allowed so Deployment Date is provided
   as a reference.*

-  User can narrow the scope of the restore from Full to Partial. For
   this lab let’s select Partial Restore from the Restore Scope
   section.\ |image118|

-  ***Note**: User can “Create Evaluation” or if urgent “Restore
   Immediately”.*

-  Select “Add” for Source Objects

-  Select “/Common/app1pool” and “Add”.

-  Verify difference between BIG-IQ and Snapshot.

    |image119|

    |image120|

-  Save and Create

-  The user can restore the partial change defined from the Snapshot
   deployment.

    |image121|

    |image122|

.. |image0| image:: media/image1.png
   :width: 1.93958in
   :height: 0.61597in
.. |image1| image:: media/image2.png
   :width: 6.42130in
   :height: 3.98644in
.. |image2| image:: media/image3.png
   :width: 6.53279in
   :height: 1.76259in
.. |image3| image:: media/image4.png
   :width: 6.50000in
   :height: 2.01820in
.. |image4| image:: media/image5.png
   :width: 6.31897in
   :height: 7.85000in
.. |image5| image:: media/image6.png
   :width: 5.63056in
   :height: 2.99033in
.. |image6| image:: media/image7.png
   :width: 2.47222in
   :height: 2.09016in
.. |image7| image:: media/image8.png
   :width: 3.38525in
   :height: 1.15301in
.. |image8| image:: media/image9.png
   :width: 6.50000in
   :height: 3.60625in
.. |image9| image:: media/image10.png
   :width: 6.50000in
   :height: 1.68889in
.. |image10| image:: media/image11.png
   :width: 5.08264in
   :height: 3.92222in
.. |image11| image:: media/image12.png
   :width: 4.32020in
   :height: 2.10656in
.. |image12| image:: media/image13.png
   :width: 6.50000in
   :height: 2.52917in
.. |image13| image:: media/image7.png
   :width: 2.47222in
   :height: 2.09016in
.. |image14| image:: media/image14.png
   :width: 4.78697in
   :height: 2.31967in
.. |image15| image:: media/image15.png
   :width: 3.07377in
   :height: 1.79768in
.. |image16| image:: media/image16.png
   :width: 4.77869in
   :height: 2.19636in
.. |image17| image:: media/image17.png
   :width: 3.31246in
   :height: 2.75083in
.. |image18| image:: media/image18.png
   :width: 4.09836in
   :height: 1.84640in
.. |image19| image:: media/image19.png
   :width: 6.27138in
   :height: 3.25000in
.. |image20| image:: media/image20.png
   :width: 6.50000in
   :height: 1.39028in
.. |image21| image:: media/image21.png
   :width: 5.69672in
   :height: 2.82593in
.. |image22| image:: media/image22.png
   :width: 3.99163in
   :height: 1.47222in
.. |image23| image:: media/image23.png
   :width: 6.49097in
   :height: 2.34236in
.. |image24| image:: media/image24.png
   :width: 4.65572in
   :height: 1.92569in
.. |image25| image:: media/image25.png
   :width: 6.50000in
   :height: 2.40619in
.. |image26| image:: media/image26.png
   :width: 6.50000in
   :height: 2.50820in
.. |image27| image:: media/image27.png
   :width: 4.68368in
   :height: 1.79508in
.. |image28| image:: media/image28.png
   :width: 2.02459in
   :height: 1.45833in
.. |image29| image:: media/image29.png
   :width: 2.40984in
   :height: 2.40984in
.. |image30| image:: media/image30.png
   :width: 4.45082in
   :height: 2.90920in
.. |image31| image:: media/image31.png
   :width: 5.20370in
   :height: 2.30328in
.. |image32| image:: media/image32.png
   :width: 2.23084in
   :height: 1.94221in
.. |image33| image:: media/image33.png
   :width: 2.32787in
   :height: 2.07099in
.. |image34| image:: media/image34.png
   :width: 6.50000in
   :height: 3.47222in
.. |image35| image:: media/image35.png
   :width: 6.49097in
   :height: 3.44444in
.. |image36| image:: media/image36.png
   :width: 6.49097in
   :height: 1.23770in
.. |image37| image:: media/image37.png
   :width: 6.48125in
   :height: 2.13934in
.. |image38| image:: media/image38.png
   :width: 6.48125in
   :height: 2.35208in
.. |image39| image:: media/image39.png
   :width: 6.50000in
   :height: 2.56557in
.. |image40| image:: media/image40.png
   :width: 6.49097in
   :height: 3.51875in
.. |image41| image:: media/image41.png
   :width: 5.74642in
   :height: 3.22131in
.. |image42| image:: media/image42.png
   :width: 5.62564in
   :height: 3.12295in
.. |image43| image:: media/image43.png
   :width: 5.33095in
   :height: 2.95082in
.. |image44| image:: media/image44.png
   :width: 5.55220in
   :height: 3.69672in
.. |image45| image:: media/image45.png
   :width: 5.78628in
   :height: 3.79508in
.. |image46| image:: media/image46.png
   :width: 5.63562in
   :height: 2.88525in
.. |image47| image:: media/image47.png
   :width: 4.54079in
   :height: 2.68033in
.. |image48| image:: media/image48.png
   :width: 5.66751in
   :height: 2.21311in
.. |image49| image:: media/image49.png
   :width: 5.33607in
   :height: 2.58012in
.. |image50| image:: media/image50.png
   :width: 5.89531in
   :height: 7.63934in
.. |image51| image:: media/image51.png
   :width: 5.71040in
   :height: 3.10656in
.. |image52| image:: media/image52.png
   :width: 5.77749in
   :height: 3.49180in
.. |image53| image:: media/image53.png
   :width: 5.79029in
   :height: 3.34426in
.. |image54| image:: media/image54.png
   :width: 6.38622in
   :height: 1.81148in
.. |image55| image:: media/image55.png
   :width: 6.08771in
   :height: 3.07377in
.. |image56| image:: media/image56.png
   :width: 5.97027in
   :height: 3.72951in
.. |image57| image:: media/image57.png
   :width: 6.50000in
   :height: 2.26458in
.. |image58| image:: media/image58.png
   :width: 3.55095in
   :height: 1.12295in
.. |image59| image:: media/image59.png
   :width: 2.77869in
   :height: 1.19931in
.. |image60| image:: media/image60.png
   :width: 4.84426in
   :height: 1.91390in
.. |image61| image:: media/image61.png
   :width: 6.50000in
   :height: 1.54375in
.. |image62| image:: media/image62.png
   :width: 3.48361in
   :height: 1.43265in
.. |image63| image:: media/image63.png
   :width: 6.50000in
   :height: 1.77569in
.. |image64| image:: media/image64.png
   :width: 6.50000in
   :height: 2.03819in
.. |image65| image:: media/image65.png
   :width: 6.50000in
   :height: 1.24931in
.. |image66| image:: media/image66.png
   :width: 1.90883in
   :height: 1.11475in
.. |image67| image:: media/image67.png
   :width: 2.24590in
   :height: 1.10238in
.. |image68| image:: media/image68.png
   :width: 5.95902in
   :height: 3.15387in
.. |image69| image:: media/image69.png
   :width: 4.90302in
   :height: 4.77049in
.. |image70| image:: media/image70.png
   :width: 6.50000in
   :height: 1.25278in
.. |image71| image:: media/image71.png
   :width: 1.60397in
   :height: 0.21872in
.. |image72| image:: media/image71.png
   :width: 1.60397in
   :height: 0.21872in
.. |image73| image:: media/image72.png
   :width: 6.50000in
   :height: 1.30833in
.. |image74| image:: media/image73.png
   :width: 6.50000in
   :height: 1.04444in
.. |image75| image:: media/image74.png
   :width: 6.50000in
   :height: 0.83611in
.. |image76| image:: media/image75.png
   :width: 3.39984in
   :height: 0.85246in
.. |image77| image:: media/image76.png
   :width: 6.50000in
   :height: 0.73333in
.. |image78| image:: media/image77.png
   :width: 5.14127in
   :height: 2.46233in
.. |image79| image:: media/image78.png
   :width: 3.51639in
   :height: 1.47646in
.. |image80| image:: media/image79.png
   :width: 6.50000in
   :height: 0.71667in
.. |image81| image:: media/image80.png
   :width: 6.50000in
   :height: 0.55903in
.. |image82| image:: media/image81.png
   :width: 2.26013in
   :height: 0.93738in
.. |image83| image:: media/image82.png
   :width: 6.50000in
   :height: 1.12500in
.. |image84| image:: media/image81.png
   :width: 2.26013in
   :height: 0.93738in
.. |image85| image:: media/image83.png
   :width: 4.62442in
   :height: 1.35400in
.. |image86| image:: media/image84.png
   :width: 3.92659in
   :height: 1.02071in
.. |image87| image:: media/image85.png
   :width: 2.45803in
   :height: 0.56243in
.. |image88| image:: media/image86.png
   :width: 6.50000in
   :height: 1.82639in
.. |image89| image:: media/image87.png
   :width: 4.93125in
   :height: 3.26643in
.. |image90| image:: media/image88.png
   :width: 2.28096in
   :height: 1.23943in
.. |image91| image:: media/image89.png
   :width: 4.59341in
   :height: 4.11475in
.. |image92| image:: media/image90.png
   :width: 4.52576in
   :height: 1.54098in
.. |image93| image:: media/image91.png
   :width: 5.94973in
   :height: 4.06557in
.. |image94| image:: media/image92.png
   :width: 1.88518in
   :height: 0.92697in
.. |image95| image:: media/image93.png
   :width: 3.62295in
   :height: 2.27173in
.. |image96| image:: media/image94.png
   :width: 1.93125in
   :height: 1.26279in
.. |image97| image:: media/image95.png
   :width: 1.93125in
   :height: 1.06679in
.. |image98| image:: media/image96.png
   :width: 6.38198in
   :height: 2.57377in
.. |image99| image:: media/image97.png
   :width: 2.82256in
   :height: 0.74991in
.. |image100| image:: media/image98.png
   :width: 1.93125in
   :height: 1.35353in
.. |image101| image:: media/image99.png
   :width: 6.49097in
   :height: 1.23125in
.. |image102| image:: media/image100.png
   :width: 5.84302in
   :height: 4.64525in
.. |image103| image:: media/image101.png
   :width: 6.50000in
   :height: 2.31250in
.. |image104| image:: media/image102.png
   :width: 6.07431in
   :height: 2.87061in
.. |image105| image:: media/image103.png
   :width: 5.35938in
   :height: 2.30328in
.. |image106| image:: media/image104.png
   :width: 6.44262in
   :height: 4.18822in
.. |image107| image:: media/image105.png
   :width: 6.50000in
   :height: 4.22917in
.. |image108| image:: media/image106.png
   :width: 3.31917in
   :height: 1.83607in
.. |image109| image:: media/image107.png
   :width: 4.76230in
   :height: 3.60734in
.. |image110| image:: media/image108.png
   :width: 6.50000in
   :height: 3.52778in
.. |image111| image:: media/image109.png
   :width: 6.50000in
   :height: 3.52778in
.. |image112| image:: media/image110.png
   :width: 6.50000in
   :height: 3.98879in
.. |image113| image:: media/image111.png
   :width: 5.45636in
   :height: 3.04098in
.. |image114| image:: media/image112.png
   :width: 5.61475in
   :height: 3.06532in
.. |image115| image:: media/image113.png
   :width: 5.19771in
   :height: 2.70492in
.. |image116| image:: media/image114.png
   :width: 6.50000in
   :height: 3.52778in
.. |image117| image:: media/image115.png
   :width: 4.70833in
   :height: 1.05460in
.. |image118| image:: media/image116.png
   :width: 5.85215in
   :height: 3.87705in
.. |image119| image:: media/image117.png
   :width: 4.22917in
   :height: 2.20722in
.. |image120| image:: media/image118.png
   :width: 6.50000in
   :height: 4.43750in
.. |image121| image:: media/image119.png
   :width: 6.50000in
   :height: 1.57292in
.. |image122| image:: media/image120.png
   :width: 4.18547in
   :height: 2.20833in
