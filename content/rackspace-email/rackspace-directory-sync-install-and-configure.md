---
node_id: 3435
title: 'Rackspace Directory Sync: Install and configure'
type: article
created_date: '2013-04-25'
created_by: Kevin Richey
last_modified_date: '2016-01-12'
last_modified_by: Stephanie Fillmon
product: Rackspace Email
product_url: rackspace-email
---

Systems Requirements
--------------------

Functional level of domain controller and Active Directory

-   Windows Server 2008
-   Windows Server 2008 R2
-   Windows Server 2012 R1 & R2

### Important notes:

-   The Directory Sync service *must be installed* *directly* on your
    domain controller.
-   After installation, Directory Sync *cannot* automatically
    synchronize existing passwords because they are unreadable from the
    Active Directory. Passwords **must be reset** after installation to
    ensure password synchronization.
-   Directory Sync is compatible with Hosted Exchange 2010, Hosted
    Exchange 2013, and Rackspace Email only.

### Software prerequisites

You must have NET Framework version 4.0 on the target domain controller
and any other domain controllers in the forest.  You can download the
appropriate .NET Framework from the [Microsoft Download
Center](http://www.microsoft.com/download/en/details.aspx?id=17718).

### Firewall ports

You do not have to open any inbound ports from the Internet to your
domain controllers.

Enable the following ports on the Directory Sync server:

-   **443** &ndash;Outbound HTTPS connections from the Directory Sync service
    to the Rackspace API.
-   **8732** &ndash; Open for connections from other domain controllers to the
    Directory Sync server not used for any connections outside
    your network. This port is used by domain controller password
    hooks.
-   **8080** &ndash; Used only locally on the Directory Sync service server
    for a web browser user interface. You can block this port for any
    external connections.

### Network encryption

Communication between Directory Sync and Rackspace is secured through
HTTPS. Communications between the Active Directory password hook and
Directory Sync is secured with Microsoft Windows Communication
Foundation (WCF) transport security which uses Windows authentication
and encryption.

### Installation files

You can find the installation files can be found while logged into
either [https://cp.rackspace.com](https://cp.rackspace.com) or into the
[https://my.rackspace.com](https://my.rackspace.com), depending on how you normally log
in.

Administrators who log in to [https://my.rackspace.com](https://my.rackspace.com), as
the primary contact can follow these steps:

1.  Click the **Products** tab and select **Email and Apps** from
    the menu.
2.  Click on your domain.

       The Directory Sync installers are located in the right hand
column.

Administrators who log in to
[https://cp.rackspace.com](https://cp.rackspace.com) with Super Admin permission can
follow these steps:

1.  While on the home page, click Domains.
2.  On the Domains Home page, click on **Tools**, and then select the
    **Directory Sync** tab.

       The Directory Sync installers are listed at the bottom of the
page.

Choose the appropriate installer, based on either 32 or 64 bit
platforms.

-   **Directory Sync Service x64.msi**
-   **Directory Sync Service x86.msi**

### Administrator&rsquo;s guide

To learn more about the features of the Directory Sync and how to use it
after installation, see the [Rackspace Directory Sync Administrator's
Guide](/how-to/rackspace-directory-sync-administrators-guide).

Install the Directory Sync service
----------------------------------

Two services are installed with the Directory Sync system, the Directory
Sync service and the Password Handler service. The Directory Sync
service is a Windows service that automatically synchronizes user
information and requires a local service account under which to run. The
Password service automatically synchronizes user password changes.

**Note: The Directory Sync service runs as the Local System account on
the domain controller.**

Copy the appropriate platform-specific Directory Sync service
**.msi** file to the domain controller. Then, open the file and follow
the prompts for installing the Directory Sync service.

1.  On the Welcome page of the Directory Sync Service Setup Wizard,
    click **Next**.
2.  On the Ready install Directory Sync Service page, click **Install**.
3.  When prompted, click **Yes** to restart your system now, or
    click **No** to manually restart it later. You must restart for the
    changes to take effect. After you restart, the installation
    wizard continues.
4.  On the Resuming the Directory Sync Service Setup Wizard page, click
    **Install**.
5.  To complete the install process, click **Finish**.
6.  When installation is complete, the web user interface (UI) for
    validation and synchronization automatically opens. A shortcut to
    the web UI is created on both the Start menu and on the desktop.

### Configure the Directory Sync service and synchronize

The Windows Services management console contains a new service called
Directory Sync. The installation automatically starts the service. If
any errors occur when the service is starting, view the event log for
more information about the error.

**Note: **We recommend creating new security groups in Active Directory
that will manage the list of synchronized users for each hosted service.
For example, if you are synchronizing Exchange users, create a new
security group in Active Directory as Rackspace Exchange or Rackspace
Hosted Exchange.

To start synchronizing Active Directory changes with Rackspace, the
Directory Sync service must be configured. Perform the following steps:

1.  Open the Directory Sync service administrative web application, if
    it is not already open:

    <img src="https://8026b2e3760e2433679c-fffceaebb8c6ee053c935e8915a3fbe7.ssl.cf2.rackcdn.com/field/image/Installer6.png" class="image-full_width" width="700" height="322" />

2.  On the **Sync Registration Page**, enter the admin ID and password
    associated with your Rackspace Cloud Office account, and then click
    **Register**.
    -   Customers who logged in to [https://my.rackspace.com](https://my.rackspace.com)
        will automatically create an admin ID through the MyRackspace
        Customer Portal before download
    -   Customers who logged in to [https://cp.rackspace.com](https://cp.rackspace.com)
        are advised to create a new admin ID dedicated to the
        sync service.

3.  On the **Settings** tab, provide the following information:

    -   **Local AD Domain**: Verify that the appropriate local Active
        Directory domain is selected.
    -   **Hosted Exchange**: Select the appropriate security group to be
        synchronized with Microsoft Exchange mailboxes.
    -   **Hosted Email**: Select the appropriate security group to be
        synchronized with Rackspace Email mailboxes.
    -   **Administrator email**: Enter an email address. All alerts will
        be sent to this email address.
    -   **Time to Send Summary Email**: Set the time when a summary
        report of changes synchronized with your Active Directory will
        be sent to the Administrator email address.  By default, this
        value is set to 08:00.

    <img src="https://8026b2e3760e2433679c-fffceaebb8c6ee053c935e8915a3fbe7.ssl.cf2.rackcdn.com/field/image/Installer7.png" class="image-full_width" width="700" height="537" />

4.  Click **Save & Start Sync** to begin a full synchronization.

    There are two types of synchronization:

    -   **Full synchronization** finds all items available for
        synchronization in the entire directory. This synchronization
        type initiates only on the first synchronization process.
    -   **Delta synchronization**finds changes available for
        synchronization in Active Directory that occurred since the
        last synchronization. This synchronization type runs
        automatically every 5 minutes by default but can also be
        performed manually. To manually run a delta synchronization,
        click on the **Sync History** tab and then click the **Sync
        Now** button.

**Note:** The Directory Sync services never makes changes to the
directory. All access is read-only.

**Synchronize users and groups**
--------------------------------

For information about how to start synchronizing your Active Directory
objects to your mailboxes and distribution lists, see the [Directory
Sync Operations
Guide](/how-to/rackspace-directory-sync-operation-guide "Directory Sync Operations Guide").

Install Password Synchronization for multiple domain controllers (optional)
---------------------------------------------------------------------------

The main installer for Directory Sync is installed on one domain
controller (DC) that will communicate directly to Rackspace. The DC
communicates through the Rackspace API to the Cloud Office Control Panel
over an HTTPS connection on port 443. This DC, or primary DC, includes
the Directory Sync UI and is where Directory Sync is configured.

If you have multiple DCs to manage the Active Directory, you must
install the Password Handler on all DCs except the primary DC (the
Password Handler is installed during initial setup). Normally, password
changes in a network occur locally and then are replicated to the other
DCs. Directory Sync is unable to see those password changes after
replication because of encryption. To ensure that password changes are
synchronized, each DC requires the Password Handler to be installed
directly. This installation requires each DC to restart.

Password changes made in the other DCs are delivered to the primary DC
over port 8732. Multiple DCs communicate internally with the primary DC
and do not send any password changes outside of the network. All
password synchronizations are funneled through the primary DC and then
synchronized to Rackspace.

The following figure illustrates this communication process.

<img src="https://8026b2e3760e2433679c-fffceaebb8c6ee053c935e8915a3fbe7.ssl.cf2.rackcdn.com/field/image/Multiple%20DC%20sync.PNG" width="1048" height="499" />

### Install Password Handler on secondary DCs

During the Installation of the Directory Sync service on the primary DC,
the **Directory Sync Password Handler Install** folder was created on
the desktop. Use the installer in this folder to synchronize your users&rsquo;
passwords across multiple domain controllers.

**Note:** the **.msi** file within the folder should be installed on the
secondary domain controllers only.

<img src="https://8026b2e3760e2433679c-fffceaebb8c6ee053c935e8915a3fbe7.ssl.cf2.rackcdn.com/field/image/Installer8_0.png" width="563" height="393" />

This process applies to multiple domain controllers (two or more).
Repeat the following steps for each additional domain controller in the
Active Directory forest.

**Important:** You must restart each domain controller to complete this
process. Consider performing these steps during off hours

1.  Copy the **.msi** file to the secondary domain controller.
2.  Double-click the installation file and click **Next** on the welcome
    page of the wizard.
3.  On the next page, click **Install**.

    After restart the installer will start up to finish
    the installation.

4.  Click **InstallFinish**.

The Password Handler of Directory Sync is installed.

**Note:** This application runs in background. You do not have to
configure any settings. Settings are taken from the primary DC.

Now that the installation is complete, see the [Rackspace Directory Sync
Administrator's
Guide](/how-to/rackspace-directory-sync-administrators-guide)
and the [Directory Sync Operations
Guide](/how-to/rackspace-directory-sync-operation-guide "Directory Sync Operations Guide")
to learn how to use Directory Sync and its features.

