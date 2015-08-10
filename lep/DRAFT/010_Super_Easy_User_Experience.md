# LEP 010 - Lantern super easy User Experience on Desktop

Date: Aug 10, 2015

Status: Draft

Author: xiam

## Abstract

## Current installation procedure

### Windows

1. The user visits https://getlantern.org.
1. If the user is on a desktop computer, the website displays a "Download"
   button that triggers a `.exe` download.
1. When the user is on a mobile device, she can provide her e-mail and a
   special service will send Lantern download links to her inbox.
1. The user downloads the `.exe` file. A new file named `lantern-installer.exe`
   is created in the `Downloads` folder.
1. The user double-clicks on the `lantern-installer.exe` file.
1. The Lantern installer shows up briefly and **something happens**.
1. Lantern is opened with no user insteraction.
1. Lantern adds itself to the system start-up sequence.
1. A new desktop shortcut is created.
1. A new start menu shortcut is created.
1. A new Lantern folder is created in the start menu.

![screen shot 2015-08-10 at 6 32 21 am](https://cloud.githubusercontent.com/assets/385670/9184225/9fda8ede-3f79-11e5-98ab-233a5b3f08ee.png)

![screen shot 2015-08-10 at 6 33 05 am](https://cloud.githubusercontent.com/assets/385670/9184226/a7042fa8-3f79-11e5-8ac1-e9cee4cd10ff.png)

![screen shot 2015-08-10 at 6 33 21 am](https://cloud.githubusercontent.com/assets/385670/9184227/a706bb1a-3f79-11e5-8f24-e45b36de7200.png)

![screen shot 2015-08-10 at 6 33 54 am](https://cloud.githubusercontent.com/assets/385670/9184239/b8e167f4-3f79-11e5-94c4-911f07548b06.png)

![screen shot 2015-08-10 at 6 34 07 am](https://cloud.githubusercontent.com/assets/385670/9184242/b8e3137e-3f79-11e5-91e9-399527ea66ac.png)

![screen shot 2015-08-10 at 6 34 16 am](https://cloud.githubusercontent.com/assets/385670/9184241/b8e29b9c-3f79-11e5-89fd-913c45e1133e.png)

![screen shot 2015-08-10 at 6 34 26 am](https://cloud.githubusercontent.com/assets/385670/9184240/b8e22d74-3f79-11e5-9b3d-773cf94cdd82.png)


### OSX

1. The user visits https://getlantern.org.
1. If the user is on a desktop computer, the website displays a "Download"
   button that triggers a `.dmg` download.
1. When the user is on a mobile device, she can provide her e-mail and a
   special service will send Lantern download links to her inbox.
1. The user downloads the `.dmg` file. A new file named `lantern-installer.dmg`
   is created in the `Downloads` folder.
1. The user double-clicks on the `lantern-installer.dmg` file and a new volume
   is created on the Desktop. This volume is named "Lantern" and has the icon.
1. The user double clicks the "Lantern" icon on the desktop and a new window
   appears. This windows has two icons, Lantern and the Applications directory.
   There is an arrow in the background.
1. The user drags the Lantern icon into the Applications directory.
1. The user starts Lantern by going to the Applications directory and looking
   for the Lantern icon or by typing "Lantern".
1. Lantern starts up  and adds itself to the system start-up sequence.

### Ubuntu Linux

1. The user visits https://getlantern.org.
1. If the user is on a desktop computer, the website displays a "Download"
   button that triggers a `.deb` download for 386 or amd64.
1. When the user is on a mobile device, she can provide her e-mail and a
   special service will send Lantern download links to her inbox.
1. The user downloads the `.deb` file. A new file named `lantern-installer.deb`
   is created in the `Downloads` folder.
1. The user double-clicks on the `lantern-installer.deb` file and the Software
   Installer shows the contents.
1. The user clicks "Install".
1. The user starts Lantern by going to the Applications directory and looking
   for the Lantern icon or by typing "Lantern".
1. Lantern starts up  and adds itself to the system start-up sequence.

## Current usage

## Proposal

This is the current download flow and I think it's great as it is:

1. The user visits https://getlantern.org.
1. If the user is on a desktop computer, the website displays a "Download"
   button that triggers the downloading of the installer. This installer is
   different for each operating system.
1. When the user is on a mobile device, she can provide her e-mail and a
   special service will send Lantern download links to her inbox.

### Windows

The Windows installation proccess is currently rather easy: no questions asked,
but also no message tells the user that Lantern is installed or how to open
Lantern again so it may be a bit confusing too.

In order to potentially avoid confusing users I propose a more classical
installation that asks for a confirmation before installing and that tells the
user that Lantern has been installed afterwards.

1. The user downloads the `.exe` file. A new file named `lantern-installer.exe`
   is created in the `Downloads` folder.
1. The user double-clicks the `lantern-installer.exe` file.
1. The installation shield asks for confirmation. "Do you want to install
   Lantern 2.0.0 on this machine? Y/N".
1. The Lantern installer shows up and a progress bar is filled. The
   installation shield stops and a message "Lantern has been installed. Thank
   you!" appears and requires a user action to be dismissed.
1. The installation process also performs the following actions:
  1. A new desktop shortcut is created.
  1. A new start menu shortcut is created.
  1. A new Lantern folder is created in the start menu.
1. After dismissing the message, Lantern is opened.
1. Lantern adds itself to the system start-up sequence.

### OSX

The OSX installation proccess would be a bit confusing for Windows users, but
it's usual stuff for OSX users.

I propose not to assume that the user that is going to install Lantern knows
how to install something in OSX and has no idea about the meaning of the
Applications directory. I also propose renaming the volume that is created by
the DMG file into "Lantern Installer" and removing the Lantern icon from it, in
order to be absolutely clear on the difference between that volume and Lantern
itself.

1. The user downloads the `.dmg` file. A new file named `lantern-installer.dmg`
   is created in the `Downloads` folder.
1. The user double-clicks on the `lantern-installer.dmg` file and a new volume
   is created on the Desktop. This volume is named "Lantern Installer" and has
   the icon of a typical volume.
1. The user double clicks the "Lantern Installer" icon on the desktop and a new
   window appears. This windows has two icons within, "Lantern" and the
   "Applications" directory.  There is an arrow in the background and a
   visibile text states clearly "Drag Lantern into the Applications directory
   to install".
1. The user drags the Lantern icon into the Applications directory.
1. The user starts Lantern by going to the Applications directory and looking
   for the Lantern icon or by typing "Lantern".
1. Lantern starts up  and adds itself to the system start-up sequence.

### Ubuntu Linux

I would not change the current Linux installation procedure. Linux users
usually figure out how to do things by themselves.

## Current day-to-day usage

The Lantern front-end is a web application that behaves the same regardless of
the operating system.

### Issue #1: IP Address and port in the address bar.

Lantern shows an IP address and port combination in the browser's address bar
which is unusual to see. The `127.0.0.1` address is due to the need to connect
to localhost in order to show Lantern's UI. We have two reasons for the odd
port: the first one if that Lantern should not interfere with any user
application that may use a common port, in this case the standard port for
serving web applications is 80; the second reason is that ports lower than 1000
require administrator privileges at least on UNIX systems.

Proposal: We can get rid of the IP by creating a especial domain name
http://my.getlantern.org that in case of being blocked it would still be
reachable through Lantern. The `my.getlantern.org` domain may not be allowed to
access websocket services on 127.0.0.1 unless a proper
[CORS](http://www.w3.org/TR/access-control/) setting is used. Unfortunately, we
won't be able to use https for my.getlantern.org, as the local websocket
service at 127.0.0.1 would also require HTTPs.

### Issue #2: Users miss the system tray icon

Some users have reported that they don't know where Lantern went after running.
Probably the system tray icon is not visible enough.

In Windows, the Lantern icon may not be even visible without clicking the
system tray arrow:

![screen shot 2015-08-10 at 3 59 12 pm](https://cloud.githubusercontent.com/assets/385670/9184100/d4b3f2ae-3f78-11e5-9cb5-1eedeae80405.png)

Proposal: Determine when the user closes all Lantern windows and show a
notification "Lantern is still running on the background. Remember that you can
manage Lantern using the system tray icon.". This notification should ideally
be above of below of the actual system tray icon. If that could not be done
easily, then a message with a picture depicting the systray could be enough.

### Issue #3: Closing the Lantern browser window does not exit Lantern.

Issue #2 would also take case of this one.

### Issue #4: Users don't know where to look for logs

If the Lantern team asks any user for the Lantern log this user must look
manually for the precise directory where the log is supposed to be.

Proposal: Add a button "View logs" in the settings dialog. This button will
open the current Lantern log in a system text editor.

## Implementation details

### Windows

Modifying the installation procedure requires changes to the the installation
script.

### OSX

Modifying the installation procedure requires changes to the DMG creation
script and to the background image.  Unfortunately the background's message
can't be localized.

### Ubuntu Linux

No changes are recommended for Linux at this time.

