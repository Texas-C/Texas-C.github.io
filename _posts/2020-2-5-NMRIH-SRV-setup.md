---
published:true
title: NMRIH server setup [EN](from official wiki)
---
(linK: https://wiki.nomoreroominhell.com/Dedicated_Server_Setup)

# Overview
----
This article covers the bare minimum for server setup. Additional guides are recommended!


* What is a "listen" server?

A listen server is run through the game's software, it's functionality that's built in.

This is ideal if you need to host a temporary server for friends to join within your city or province/state.

> Jump to the article: Listen_Server_Setup


* What is a "dedicated" server?

A dedicated server is technicaly a server that is availiable all the time, day or night, 24/7, 365. To do this it has a dedicated machine to use.

Special hardware should be used to run a dedicated server the right way. However, since it's just another piece of software you could use a spare computer.

> Jump to the article: Dedicated_Server_Setup

# Connectivity
----
> Note:	This article details port forwarding in a basic manor. If you’re familiar, you can skip it.

To get things started, we should talk about what these "external" and "internal" IP addresses are, and how they affect you.

You will probably have a DHCP server in your house/apartment. Most of the time this will be your router, but in some cases it could be something else. DHCP is a system that allows the router (or other device) to pass out internal IP addresses to other devices (such as your computer, laptop or tablet) that connect to it. Inside your network, all the devices talk to each other by using these internal addresses. Only devices inside your network can talk to others with these addresses - as they are reserved for use in LAN networks.


> Note:	You can tell a LAN network ip from a public one as LAN IPs tend to start with 192.168

If your friend wanted to connect to your NMRiH server, they may see it having the ip 192.168.1.2. This is what you see too, but your friend will scream and kick at not being able to connect. Just like you, your friend will be using these internal addresses as well. The problem here is that they correspond to computers/devices on their network, so when they try to connect, their router will only search on their network for a device that has the 192.168.1.2 address. In order to solve this problem, this is where external ip addresses come in.

Your router is assigned a public (external) IP address by your ISP's modem so it can communicate to computers around the word. This public address is also used when someone wants to give you data (websites, game servers, etc.). You can tell your router to pass on information it gets on a "port" to a computer inside the network. This is done by specifying the IP of the machine on the network, and what port to send it to. The common term for this is called "Port Forwarding", as it forwards the port(s) to another computer/device. This article won't go into detail about how to do that, but a great place to start is http://www.portforward.com, which will show you how to port forward.

To play on the server you only need to have 27015 (UDP) open/forwarded. The server doesn't use TCP traffic on this port for gameplay and thus is recommended that you do not forward it.


> Note:	While port forwarding is a quick and easy way to do this, it can be easily "messed up".

Now remember reading before about DHCP? It will come back to haunt you sadly. Since there is a limited amount of internal IP addresses, the router will try free up internal IP address for use. It will do this by removing unused addresses. These include computers/devices that are not on. So if you happen to turn the computer off that hosts the server and another device connects, it is likely to take that address your computer had and thus breaking the port forwarding you did. In order to prevent this from happening, you will have to set a static IP for the machine (technically the machine's ethernet interface).

That should now all be clear, you may be wondering where to get your external/public IP from. There are many sites that show you it - even Google "What is my IP address". After obtaining it, send it to your friends.


Now, many will argue at why you only need 27015 (UDP) open. Why not 27015 (TCP) with all those other ports as well? The remote console (RCON) for the server runs on 27015 (TCP) and can lead to security issues and Denial of Service attacks (DoS). You should only allow the TCP traffic in if you know what you're doing. Other ports shown in other guides are not needed, and thus don't need to be opened.

# Setting up a Dedicated Server
---
Dedicated servers are preferred if you want to ensure you get the best performance and flexibility. Most of the time, it's installed on a separate machine/computer where both it and the server are made available 24/7 (hence the term dedicated).


> Note:	You cannot use a steam account in SteamCMD and the Steam Client concurrently. You should register a separate account for the exclusive use with SteamCMD.

## Windows
This section will outline setup and administration for the latest generation of Windows. Versions Server 2003, XP and lesser will not directly follow these steps.

You will require the core SteamCMD files. You can find them here: https://steamcdn-a.akamaihd.net/client/installer/steamcmd.zip

Extract the files to a directory other then the steam client. Now open a command prompt to that directory and run steamcmd.


> Warning!	Windows might give you an "Error: Steam needs to be online to update. Please confirm your network connection and try again." or "Error: Download failed: http error 0" error if Internet Explorer is being anal.

>This is usually fixed by checking "Automatically detect settings" in IE (Internet Explorer) through the lan settings in the Internet option menu.
>1. Open Internet Explorer (IE)
>2. Click on Tools > Internet Options
>3. Click on the "Connections Tab"
>4. At the Bottom, you should see "Local Area Network (LAN) Settings".
>5. Check the first box "Automatically detect settings"
>6. Hit OK, and apply. Try running the SteamCMD again, if it still doesn't work. try lowering your "Internet Security level zone" to medium or lower. You can find that in the "Security" tab in "Internet Options".

SteamCMD will update if required each time it executed. Now to download the NMRiH dedicated server software, Type the following into SteamCMD and hit enter after each line. (be mindful of the spaces)

    login anonymous
    force_install_dir .\nmrih_ds\
    app_update 317670
    
Now you should be able to navigate to the srcds.exe file and use: `srcds.exe -console -game nmrih -insecure +map nmo_chinatown`

__Windows - Persistent Dedicated Server Login__
The following command can be used to provide your dedicated server with a persistent login, which allows users to "favorite" your server as it uses a registered domain name's DNS entry to find your server.

    sv_setsteamaccount

The following steps will need to be taken:

    1. Go to Steam website: https://steamcommunity.com/dev/managegameservers and login to your steam account.
    2. Fill the App ID field with the ID of the base game (224260 for NMRiH) and the Memo field with a text to keep track of what token belongs to what game (nmrihserver for example, remember that each server should have its unique token), then press Create.
    3. Copy the Login Token, then open autoexec.cfg in you nmrih cfg folder and add 'sv_setsteamaccout XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX' without quotes. Save the file.
    4. Start your server.
    5. Server console now shows: Assigned persistent gameserver Steam ID [G:1xxx].
    
Alternatively you can add +sv_setsteamaccount XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX to your command line when running the server via srcds.

For a more in-depth information of what the Game Server Login Token (GSLT) is and does visit LinuxGSM - What is GSLT? and Steam Support - CS:GO GSLT Bans

# Linux
----
No More Room in Hell server is available on Linux. These are brief instructions on how to install and start a server.

For further assistance take a look at the Server Hosting Forum. http://www.nomoreroominhell.com/forums/index.php?showforum=70

## Installation
Create a username for your server.

> Warning!	For security best practice do not run your server a root.

    adduser nmrihserver
    
Chose a directory to install the server.

    cd /home/nmrihserver
    
Create a directory for steamCMD.

    mkdir steamcmd
    cd steamcmd

Download SteamCMD.

    wget http://media.steampowered.com/client/steamcmd_linux.tar.gz

Extract steamCMD.

    tar -xvzf steamcmd_linux.tar.gz

Create a directory for the server files,

    mkdir /home/nmrihserver/serverfiles

Run the following command to download the server. You will need a Steam username and password to authenticate with steamCMD.

> Warning!	For security reasons it is recommended that you create a new Steam username just for the server.

    ./steamcmd.sh +login username password +force_install_dir "/home/nmrihserver/serverfiles" +app_update 317670 validate +quit

__64-Bit Distros__
The No More Room in Hell server requires 32-bit library's to run. You are required to install extra packages to run this server.

* Ubuntu 64-Bit

    sudo apt-get install lib32gcc1
    
* Debian 64-Bit

    dpkg --add-architecture i386
    apt-get update && apt-get install lib32gcc1

* CentOS 64-Bit

    yum install glibc.i686 libstdc++.i686

* Other Distros

Other 64-bit distros will have similar requirements to this. It is recommended that you seek out documentation or forums posts regarding 32-bit libraries.

## Starting the server
to run the server use the following command.

    ./srcds_run -game nmrih +map nmo_broadway -maxplayers 8 -autoupdate
    
There are various command line parameters available for the server. A complete list of parameters are available from the Valve Wiki.

https://developer.valvesoftware.com/wiki/Command_Line_Options#Source_Dedicated_Server

## GLIBC_2.15 error
No More Room in Hell requires GLIBC_2.15 or above to run. If your distro does not meet this requirement you may get the following error.

    ./srcds_nmrih: /lib/i386-linux-gnu/i686/cmov/libm.so.6: version `GLIBC_2.15' not found

To fix this download the following file to the directory with srcds_nmrih included within it.

    wget https://github.com/dgibbs64/linuxgameservers/raw/master/NoMoreRoomInHell/dependencies/libm.so.6

srcds_nmrih will use this updated version of libm.so.6 instead of the OS version. This problem commonly effects distros with a long life cycle such as RHEL and CentOS.

# Server Commands
----
You can use these commands on listen servers as well as dedicated servers. Enter the command/cvar you want to use, and the value (if it requires one).


    sv_votekick_timer 10


> While on a listen server, open the developer console with the ~ key.

Command/Cvar    | Description	| Example
----------------|--------------|-------------
changelevel     |Peacefuly change the map to another, and tell clients it's changing.	| changelevel nms_northway
map	            |Force the server to change the map to another. Used to reload configuration as it kicks all clients (Server Shutting Down).	| map nms_northway
kick	| Kick a player right away.	| kick dark_st3alth

