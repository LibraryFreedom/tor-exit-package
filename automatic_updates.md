It is **very important**  to keep your Tor relay current with the latest
security (and other) updates automatically. This guide will help you
setup automatic updates on Debian based systems.

Step 0: Make sure you've installed Tor from official repository
-------
Various operating systems have their own package repositories and that
often translates to them having ancient versions of software packaged,
and not the most recent one. This is the case for Ubuntu while Debian
has the most recent packages.

To be sure you always have the latest Tor version installed on your
system, it's important to get the package from official Tor repository.

Follow this instruction to complete this step:
https://www.torproject.org/docs/debian.html.en#ubuntu

Step 1: Install automatic updates
-------
Make sure your system is up to date

    $ sudo apt-get update && sudo apt-get upgrade

Install `unattended-upgrades` `apt-listchanges` packages

    $ sudo apt-get install unattended-upgrades apt-listchanges

Step 2.a: Configure automatic updates
-------
Open the following file in your text editor

    $ sudo editor /etc/apt/apt.conf.d/50unattended-upgrades

This section controls which package are being updated. The defaults
include all the security updates.

    Unattended-Upgrade::Origins-Pattern {
        // ...
    };

All we need to do is to let it update Tor as well. Add the following
line before `};`

    "origin=TorProject"

Step 2.b: Make sure you're notified if an error occures
-------
Add the following lines at the end of your config file

    Unattended-Upgrade::Mail "root";
    Unattended-Upgrade::MailOnlyOnError "true";

Step 3: Turn on automatic updates
-------
Run the following command to enable automatic updates

    $ sudo cp /usr/share/unattended-upgrades/20auto-upgrades /etc/apt/apt.conf.d/20auto-upgrades

Step 4: Test your settings
-------
To make sure everything works smoothly without any errors, run automatic
updates in simulation mode

    $ sudo unattended-upgrades --dry-run

If you've misconfigured something, you'll get an error here. Otherwise,
you've successfully configured automatic updates.

If you get an error, go back to step 2 and make sure you've done
everything correctly.

**NOTE**: As an alternative to step 2.a and 2.b, you can download our 
pre-made config. You must be aware that this config upgrades all
packages from every origin. In order to do so, run the following commands:

    $ sudo wget -p -O /etc/apt/apt.conf.d/50unattended-upgrades https://raw.githubusercontent.com/LibraryFreedom/tor-exit-package/master/automatic-updates/50unattended-upgrades
  
Step 5: Monitor your logs
-------
Remember to monitor your logs and mails being sent to root. 
Sometimes you've to install packages manually.
`unattended-upgrades` logs are usually stored at `/var/log/unattended-upgrades/unattended-upgrades.log`
