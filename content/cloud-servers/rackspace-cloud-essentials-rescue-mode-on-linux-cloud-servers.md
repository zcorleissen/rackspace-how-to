---
node_id: 3295
title: Rackspace Cloud Essentials - Rescue Mode on Linux Cloud Servers
type: article
created_date: '2013-02-11'
created_by: Renee Rendon
last_modified_date: '2015-07-16'
last_modified_by: Rose Contreras
product: Cloud Servers
product_url: cloud-servers
---

If your Linux system has become non-bootable or is suffering from critical system errors, you can use rescue mode to recover your system data. These problems may be caused by file system corruption, boot file corruption, or other configuration errors. Normally, if your system encounters any problem during the boot process, you would boot in to a maintenance mode environment known as Single User Mode that would allow you to login with your root password and check for any errors. Using Single User Mode can cause the following issues:

- Your system is read-only and you cannot make corrective changes. Most services such as networking are disabled. This would prevent you from copying your data to another server.

- You would have to access your server using the Console, which is slower than using a traditional SSH login.

To avoid having to use single user mode, start your server up in rescue mode in the Cloud Control Panel.

<a id="What_is_Rescue_mode.3F" name="What_is_Rescue_mode.3F"> </a>

<table class="toc" id="toc" summary="Contents">
	<tbody>
		<tr>
			<td>
			<h3>Contents</h3>

			<ul>
				<li class="toclevel-1"><span class="toctext"><a href="/how-to/">Rescue mode</a></li>
				<li class="toclevel-1"><a href="/how-to/"><span class="toctext">Put your server into rescue mode</a></li>
				<li class="toclevel-1"><a href="/how-to/"><span class="toctext">Connect to your server in rescue mode</a></li>
				<li class="toclevel-1"><a href="/how-to/"><span class="toctext">Troubleshoot your server in rescue mode</a></li>
				<li class="toclevel-1"><a href="/how-to/"><span class="toctext">Exitrescue mode</a></li>
			</ul>
			</td>
		</tr>
	</tbody>
</table>

## Rescue mode

Rescue mode grants the root user full access to your non-bootable server&rsquo;s filesystem. You can use it to modify problems in configuration files or to copy data from your Cloud Server to a remote location. Rescue Mode through the Cloud Control Panel is similar to booting into single-user mode with networking enabled.

## Put your server into rescue mode

1. Log in to the Cloud Control Panel and select **Servers**.

2. Click the gear menu next to the server and select **Enter Rescue Mode**.

3. Read the information in the pop-up window, then press **Enter Rescue Mode**.

4. Copy the temporary password and click **Dismiss Password**.

    <img alt="" border="1" height="263" src="https://8026b2e3760e2433679c-fffceaebb8c6ee053c935e8915a3fbe7.ssl.cf2.rackcdn.com/field/image/Feb%2011%20-%20Rescue%20Mode%20Activated.png" width="440" />

    **NOTE:** The green bar to the left of the server name will turn yellow during the process of building into Rescue Mode and then red when the process has completed. This may take several minutes.

    <img alt="" border="1" height="82" src="https://8026b2e3760e2433679c-fffceaebb8c6ee053c935e8915a3fbe7.ssl.cf2.rackcdn.com/field/image/Feb%2011%20-%20Yellow%20Bar.png" width="192" />
	<img alt="" border="1" height="87" src="https://8026b2e3760e2433679c-fffceaebb8c6ee053c935e8915a3fbe7.ssl.cf2.rackcdn.com/field/image/Feb%2011%20-%20Red%20Box.png" width="172" />

5. Once your server is in rescue mode, use an SSH client to connect to your server using the public IP address and the temporary root password to log in to rescue mode.

## Troubleshoot your server in rescue mode

Before you can access the files on your server, you must mount the server's file system. To do that, look at your partitions to determine your file system's device.

**NOTE:** If you plan to use <code>fsck</code> on this filesystem, DO NOT MOUNT the filesystem.

1. Log into your server in rescue mode and run the following command:

        fdisk -l

    You will get the following output:

    <img alt="" border="1" height="537" src="https://8026b2e3760e2433679c-fffceaebb8c6ee053c935e8915a3fbe7.ssl.cf2.rackcdn.com/field/image/fdisknew.png" width="750" />

    Note the different disk names. The disk entry will display the device and size of the disk. For example: Disk /dev/xvdb1: 20 GiB

    The first block, /dev/xvdb1, with size 20 GiB, is the rescue mode filesystem.

    The second block, /dev/xvda1, with size of 20 GiB, is the server's file system. The size will vary depending on the size of your server.

    If a third block is displayed, it is the swap volume. **NOTE:** This only occurs only on older cloud servers).

2. When you've identified the block for your server's file system, note the part after <code>disk</code> that looks like a file path. In the example above, the device is <code>/dev/xvda1</code>.  The device will vary depending on the distribution image used to build your server. Now that you know your file system's device, you can assign it a directory and mount it for access. Plug your file system device into the following command in place of <code>/dev/diskdevice</code>.

        mount /dev/diskdevice /mnt

3. For example, for /dev/sda1 the command would be:</li>

        mount /dev/xvda1 /mnt

4. Now you can access your files through the <code>/mnt</code> directory. Just remember that you'll need to put "/mnt" in front of the usual paths you'd use to get to files. For example, if you have a problem in the /etc/fstab file you need to fix, you'd actually access that file at:

        /mnt/etc/fstab

5. If you just edit <code>/etc/fstab</code> while in rescue mode, you will change the <code>fstab</code> for the rescue mode file system, not your normal file system.

## Exit rescue mode

Once you are done troubleshooting your system, click **Exit Rescue Mode** in the Cloud Control Panel on your Server Details page.

<p>&nbsp;</p>
