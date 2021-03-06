---
aliases:
- /mackit/troubleshoot/
description: Troubleshooting, logs, and known issues
keywords:
- mac, troubleshooting, logs, issues
menu:
  main:
    identifier: docker-mac-troubleshoot
    parent: pinata_mac_menu
    weight: 6
title: Logs and Troubleshooting
---

#  Logs and Troubleshooting

- [Logs and Troubleshooting](#logs-and-troubleshooting)
	- [Docker Knowledge Hub](#docker-knowledge-hub)
	- [Diagnose problems and send feedback](#diagnose-problems-and-send-feedback)
- [Checking the logs](#checking-the-logs)
	- [Use the command line to view logs](#use-the-command-line-to-view-logs)
	- [Use the Mac Console for log queries](#use-the-mac-console-for-log-queries)
- [Troubleshooting](#troubleshooting)
	- [Recreate or update your containers after Beta 18 upgrade](#recreate-or-update-your-containers-after-beta-18-upgrade)
	- [Incompatible CPU detected](#incompatible-cpu-detected)
	- [Workarounds for common problems](#workarounds-for-common-problems)
- [Known issues](#known-issues)

## Docker Knowledge Hub

**Looking for help with Docker for Mac?** Check out the [Docker Knowledge Hub](http://success.docker.com/) for knowledge base articles, FAQs, and technical support for various subscription levels.

## Diagnose problems and send feedback

If you encounter problems for which you do not find solutions in this documentation, [Docker for Mac issues on GitHub](https://github.com/docker/for-mac/issues) already filed by other users, or on the [Docker for Mac forum](https://forums.docker.com/c/docker-for-mac), we can help you troubleshoot the log data.

Choose <img src="../images/whale-x.png"> --> **Diagnose & Feedback** from the menu bar.

Select **Open Issues**.

This uploads (sends) the logs to Docker, and opens [Docker for Mac issues on GitHub](https://github.com/docker/for-mac/issues/) in your web browser in a "create new issue" template.

![Diagnostics & Feedback](images/settings-diagnose-id.png)

The issue is prepopulated with the ID and summary of the diagnostic you just ran, along with system and version details, and a template for you to fill in a description of expected and actual behavior, and steps to reproduce the issue.

![Create issue on GitHub](images/diagnose-issue.png)

You can also create a new issue by going to [Docker for Mac issues on GitHub](https://github.com/docker/for-mac/) and following the instructions in the README. Click [open a new issue](https://github.com/docker/for-mac/issues/new) on that page (or right here &#9786;) to get a "create new issue" template prepopulated with sections for the ID and summary of your diagnostics, system and version details, description of expected and actual behavior, and steps to reproduce the issue.

![issue template](images/diagnose-d4mac-issues-template.png)

<a name="logs"></a>
## Checking the logs

In addition to using the diagnose and feedback option to submit logs, you can browse the logs yourself.

#### Use the command line to view logs

To view Docker for Mac logs at the command line, type this command in a terminal window or your favorite shell.

    $ syslog -k Sender Docker

Alternatively, you can send the output of this command to a file. The following command redirects the log output to a file called `my_docker_logs.txt`.

    $ syslog -k Sender Docker > ~/Desktop/my_docker_logs.txt

#### Use the Mac Console for log queries

Macs provide a built-in log viewer. You can use the Mac Console System Log Query to check Docker app logs.

The Console lives on your Mac hard drive in `Applications` > `Utilities`. You can bring it up quickly by just searching for it with Spotlight Search.

To find all Docker app log messages, do the following.

1. From the Console menu, choose **File** > **New System Log Query...**

    ![Mac Console search for Docker app](images/console_logs_search.png)

    * Name your search (for example `Docker`)
    * Set the **Sender** to **Docker**

2. Click **OK** to run the log query.

  ![Mac Console display of Docker app search results](images/console_logs.png)

You can use the Console Log Query to search logs, filter the results in various ways, and create reports.

For example, you could construct a search for log messages sent by Docker that contain the word `hypervisor` then filter the results by time (earlier, later, now).

The diagnostics and usage information to the left of the results provide auto-generated reports on packages.

<a name="troubleshoot"></a>
## Troubleshooting

#### Recreate or update your containers after Beta 18 upgrade

Docker 1.12.0 RC3 release introduces a backward incompatible change from RC2 to RC3. (For more information, see https://github.com/docker/docker/issues/24343#issuecomment-230623542.)

You may get the following error when you try to start a container created with pre-Beta 18 Docker for Mac applications.

			Error response from daemon: Unknown runtime specified default

You can fix this by either [recreating](#recreate-your-containers) or [updating](#update-your-containers) your containers.

If you get the error message shown above, we recommend recreating them.

##### Recreate your containers

To recreate your containers, use Docker Compose.

			docker-compose down && docker-compose up

##### Update your containers

To fix existing containers, follow these steps.

1. Run this command.

			$ docker run --rm -v /var/lib/docker:/docker cpuguy83/docker112rc3-runtimefix:rc3

			Unable to find image 'cpuguy83/docker112rc3-runtimefix:rc3' locally
			rc3: Pulling from cpuguy83/docker112rc3-runtimefix
			91e7f9981d55: Pull complete
			Digest: sha256:96abed3f7a7a574774400ff20c6808aac37d37d787d1164d332675392675005c
			Status: Downloaded newer image for cpuguy83/docker112rc3-runtimefix:rc3
			proccessed 1648f773f92e8a4aad508a45088ca9137c3103457b48be1afb3fd8b4369e5140
			skipping container '433ba7ead89ba645efe9b5fff578e674aabba95d6dcb3910c9ad7f1a5c6b4538': already fixed
			proccessed 43df7f2ac8fc912046dfc48cf5d599018af8f60fee50eb7b09c1e10147758f06
			proccessed 65204cfa00b1b6679536c6ac72cdde1dbb43049af208973030b6d91356166958
			proccessed 66a72622e306450fd07f2b3a833355379884b7a6165b7527c10390c36536d82d
			proccessed 9d196e78390eeb44d3b354d24e25225d045f33f1666243466b3ed42fe670245c
			proccessed b9a0ecfe2ed9d561463251aa90fd1442299bcd9ea191a17055b01c6a00533b05
			proccessed c129a775c3fa3b6337e13b50aea84e4977c1774994be1f50ff13cbe60de9ac76
			proccessed dea73dc21126434f14c58b83140bf6470aa67e622daa85603a13bc48af7f8b04
			proccessed dfa8f9278642ab0f3e82ee8e4ad029587aafef9571ff50190e83757c03b4216c
			proccessed ee5bf706b6600a46e5d26327b13c3c1c5f7b261313438d47318702ff6ed8b30b

2. Quit Docker.

3. Start Docker.

	> **Note:**  Be sure to quit and then restart Docker for Mac before attempting to start containers.

4. Try to start the container again:

				$ docker start old-container
				old-container

#### Incompatible CPU detected

Docker for Mac requires a processor (CPU) that supports virtualization and, more specifically, the [Apple Hypervisor framework](https://developer.apple.com/library/mac/documentation/DriversKernelHardware/Reference/Hypervisor/). Docker for Mac is only compatible with Macs that have a CPU that supports the Hypervisor framework. Most Macs built in 2010 and later support it, as described in the Apple Hypervisor Framework documentation about supported hardware:

*Generally, machines with an Intel VT-x feature set that includes Extended Page Tables (EPT) and Unrestricted Mode are supported.*

To check if your Mac supports the Hypervisor framework, run this command in a terminal window.

```
sysctl kern.hv_support
```
If your Mac supports the Hypervisor Framework, the command will print `kern.hv_support: 1`.

If not, the command will print `kern.hv_support: 0`.

See also, [Hypervisor Framework Reference](https://developer.apple.com/library/mac/documentation/DriversKernelHardware/Reference/Hypervisor/) in the Apple documentation, and Docker for Mac system requirements in [What to know before you install](index.md#what-to-know-before-you-install).


#### Workarounds for common problems

* If Docker for Mac fails to install or start properly:
  * Make sure you quit Docker for Mac before installing a new version of the application ( <img src="../images/whale-x.png"> --> **Quit Docker**). Otherwise, you will get an "application in use" error when you try to copy the new app from the `.dmg` to `/Applications`.

  * Restart your Mac to stop / discard any vestige of the daemon running from the previously installed version.

  * Run the the uninstall commands from the menu.

* If `docker` commands aren't working properly or as expected:

    Make sure you are not using the legacy Docker Machine environment in your shell or command window. You do not need `DOCKER_HOST` set, so unset it as it may be pointing at another Docker (e.g. VirtualBox). If you use bash, `unset ${!DOCKER_*}` will unset existing `DOCKER` environment variables you have set. For other shells, unset each environment variable individually as described in [Docker for Mac vs. Docker Toolbox](docker-toolbox.md#setting-up-to-run-docker-for-mac).

* Note that network connections will fail if the OS X Firewall is set to
"Block all incoming connections". You can enable the firewall, but `bootpd` must be allowed incoming connections so that the VM can get an IP address.

* For the `hello-world-nginx` example, Docker for Mac must be running in order to get to the webserver on `http://localhost/`.

    Make sure that the Docker whale is showing in the menu bar, and that you run the Docker commands in a shell that is connected to the Docker for Mac Engine (not Engine from Toolbox). Otherwise, you might start the webserver container but get a "web page not available" error when you go to `localhost`.  For more on distinguishing between the two environments, see "Running Docker for Mac and Docker Toolbox" in [Getting Started](index.md).

* If you see errors like `Bind for 0.0.0.0:8080 failed: port is already allocated` or
  `listen tcp:0.0.0.0:8080: bind: address is already in use`:

	  These errors are often caused by some other software on the Mac using those ports.
		Run `lsof -i tcp:8080` to discover the name and pid of the other process and
		decide whether to shut the other process down, or to use a different port in
		your docker app.

See also <a href="#issues">Known Issues</a>.

<a name="issues"></a>
## Known issues

* You might encounter errors when using `docker-compose up` with Docker for Mac (`ValueError: Extra Data`). We've identified this is likely related to data and/or events being passed all at once rather than one by one, so sometimes the data comes back as 2+ objects concatenated and causes an error.

* Force-ejecting the `.dmg` after running `Docker.app` from it results in an unresponsive whale in the menu bar, Docker tasks "not responding" in activity monitor, helper processes running, and supporting technologies consuming large percentages of CPU. Please reboot, and then re-start Docker for Mac. If needed,`force quit` any Docker related applications as part of the reboot.

* Docker does not auto-start on login even when it is enabled in <img src="../images/whale-x.png"> --> **Preferences**. This is related to a set of issues with Docker helper, registration, and versioning.

* Docker for Mac uses the `HyperKit` hypervisor (https://github.com/docker/hyperkit) in Mac OS X 10.10 Yosemite and higher. If you are developing with tools that have conflicts with `HyperKit`, such as [Intel Hardware Accelerated Execution Manager (HAXM)](https://software.intel.com/en-us/android/articles/intel-hardware-accelerated-execution-manager/), the current workaround is not to run them at the same time. You can pause `HyperKit` by quitting Docker for Mac temporarily while you work with HAXM. This will allow you to continue work with the other tools and prevent `HyperKit` from interfering.

*  If you are working with applications like [Apache Maven](https://maven.apache.org/) that expect settings for `DOCKER_HOST` and `DOCKER_CERT_PATH` environment variables, specify these to connect to Docker instances through Unix sockets. For example:

        export DOCKER_HOST=unix:///var/run/docker.sock

* `docker-compose` 1.7.1 performs DNS unnecessary lookups for `localunixsocket.local` which can take 5s to timeout on some networks. If `docker-compose` commands seem very slow but seem to speed up when the network is disabled (e.g. when disconnected from wifi), try appending `127.0.0.1 localunixsocket.local` to the file `/etc/hosts`.
Alternatively you could create a plain-text TCP proxy on localhost:1234 using:

        docker run -d -v /var/run/docker.sock:/var/run/docker.sock -p 127.0.0.1:1234:1234 bobrik/socat TCP-LISTEN:1234,fork UNIX-CONNECT:/var/run/docker.sock

  and then `export DOCKER_HOST=tcp://localhost:1234`.

* There are a number of issues with the performance of directories
  bind-mounted with `osxfs`.  In particular, writes of small blocks, and
  traversals of large directories are currently slow.  Additionally,
  containers that perform large numbers of directory operations, such as
  repeated scans of large directory trees, may suffer from poor
  performance. Applications that behave in this way include:

  - `rake`
  - `ember build`
  - Symfony
  - Magento

  As a work-around for this behavior, you can put vendor or third-party
  library directories in Docker volumes, perform temporary file system
  operations outside of `osxfs` mounts, and use third-party tools like
  Unison or `rsync` to synchronize between container directories and
  bind-mounted directories. We are actively working on `osxfs`
  performance using a number of different techniques and we look forward
  to sharing improvements with you soon.

* If system does not have access to an NTP server then after a hibernate the time seen by Docker for Mac may be considerably out of sync with the host. Furthermore the time may slowly drift out of sync during use. The time can be manually reset after hibernation by running:

        docker run --rm --privileged alpine hwclock -s

  Or alternatively to resolve both issues you may like to add the local clock as a low priority (high stratum) fallback NTP time source for the host by editing the host's `/etc/ntp-restrict.conf` to add:

        server 127.127.1.1              # LCL, local clock
        fudge  127.127.1.1 stratum 12   # increase stratum

  And then restarting the NTP service with:

        sudo launchctl unload /System/Library/LaunchDaemons/org.ntp.ntpd.plist
        sudo launchctl load /System/Library/LaunchDaemons/org.ntp.ntpd.plist

<p style="margin-bottom:300px">&nbsp;</p>
