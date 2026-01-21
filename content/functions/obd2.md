---
title: "OBD2 Setup"
description: "How to configure OBD2 system"
---

This document outlines the setup procedure for the OBD2 function in the Mainline CCS
Software.

A generic **“OBD2”** Can cable is supplied with your dyno. This cable is a “straight thru”
non electronic cable, meaning it is just a direct connection to the Vehicle CAN Bus via
the OBD2 socket, there is no “electronics” inside the cable such as what there would be
on an “Elm327” style of cable that would plug into a USB port or use Bluetooth. ONLY a
Mainline Supplied OBD2 cable can be used and is to be plugged into the CAN 4 A or 4B
sockets of the CAN hub.

The OBD2 Function in the Mainline CCS software is derived to be a somewhat “User
Definable” set of PIDs to access/log from the test vehicle. A generic set of PIDs may be
included with the installed software, and “installing “ is covered below.

Firstly, go to the `Communications` Tab – Click on `Accessory Can1` – `Settings`, and make
sure the `Bit Rate` is set to `500kps`.

![alt text](../../img/obd2/0538daaa5583c23e17db3a12f2281a55_img.jpg)

After the `Bit Rate` has been set on `Accessory Can1`, click on the `Communications` Tab – `Accessory Can1` – `OBDII`.

On the `OBDII` tab, click the `Enable` check Box, set a default `Request Interval` to `200 ms`.

![alt text](../../img/obd2/61972112770d9c8fb00883057954f885_img.jpg)

After Enabling and setting a `Request Interval`, click the ‘Add” button at the bottom left of
the screen:

![alt text](../../img/obd2/d5fc881e4328d6a2e76c9576408ced49_img.jpg)

The screen will now look like this:

![alt text](../../img/obd2/cb00037bd3b3af9720d5551ad2f818dd_img.jpg)

Now click the Import Service Button.

You will then be prompted to select an “OBD2 Service” set from a folder on the PC,
generally you would find the file `standard.obd2svc` located in the `C:\Drivers\CCS
folder`, but it may be in the Documents folder, or you may be able to download it from the
Mainline Download Server (available March 2026), or it may be emailed to and you can
save it anywhere you wish on the Dyno PC. The example shown below the file was
located in `C:\Drivers\CCS System\OBD2` for example:

![alt text](../../img/obd2/7e2f2d03a5dda38b038fd4884629a2b4_img.jpg)

Once the “Service” has been loaded, the screen will change and show the PIDs that are
setup withing the “Service File”:

![alt text](../../img/obd2/48f188337e3ba41df38fab9ac0afb1bd_img.jpg)

The Standard OBD2 service ?le only has the first 20 generic OBDII PIDs,

Essentially once all these steps have been completed, the Dyno Software should be
polling the test vehicle CAN bus for OBDII data. The PIDs shown in the Standard Service
may not be all available on the test vehicle. Once connected and polling for PIDs/Data
the Supported PIDs box will show the PIDs supported by the test vehicle:

![alt text](../../img/obd2/ddfe517a5ad9c77c89a57a5e780b24ca_img.jpg)

The above example is from a 2023 Ford Ranger, and many of these PIDs are well beyond
the Standard Service PIDs. A user can generate their own custom OBDII “Services” for
specific vehicles, and these “Services” can be Added or Removed when needed.

For best results, due to the way OBDII operates from a polling and data receive point of
view, the lowest amount of PIDs “polled” will return the highest Poll rate.

Users of CCS system could if wanted, share OBDII services, as the ability certainly
exists for advanced users to write OBDII Services to acquire Manufacturer Specific PIDs
on Mode 21 or Mode 22 for example. The Standard Service just deals with Mode 1.

Further information on the Topic of OBDII can be found here
https://en.wikipedia.org/wiki/OBD-II_PIDs

There have been 2 sorts of OBD2 cables supplied, the blue version was an earlier
version where the end was removable so the cable could be used as “Generic” CAN
cable, or the later OBD2 only end version:

![alt text](../../img/obd2/4b9457ad2400572dbf0c0c9f7c825643_img.jpg)

![alt text](../../img/obd2/29d638143e0fbfb368ab967dac226c2a_img.jpg)
