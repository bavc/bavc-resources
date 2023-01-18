---
layout: page
title: SAN Maintenance
parent: Technical Documentation
---

# SAN Maintenance

Our SymplyUltra SAN is very sturdy, and doesn't often need repair or maintenance. However, you should be checking in on it now and again to make sure it's working properly.

## Visual Inspection

Set a periodic reminder to check the front of the SAN. There should be only blue lights, and no red lights. If you see any red lights then something is wrong and you will have to check the GUI maintenance portal for more information

## GUI Maintenance Portal

The online GUI has more information about the SAN. It logs errors, shows the status of the drives, and allows you to add more SAN clients to the network.

To log into the GUI you'll need to navigate to the IP address and log in. This information can all be found in 1password, under the entry _SAN Maintenance GUI_. In this entry there are two IP addresses, one for each SAN controller. These are redundant, you can use either. You will have to accept a security risk, don't worry about this. It also might get mad at your browser, but that shouldn't be a problem. It may ask you to download a version of Flash Player, this shouldn't be necessary.

### SAN and Drive Status

Once logged in, you check the status of the SAN and the drives by pressing the _System_ button on the right side of the page. From here you can hover over each drive bay to see the status. Any problematic drives will be highlighted in red.

### Alarms

At the top of the page you'll see a counter for _Critical_, _Major_, and _Warning_ errors. It's good to periodically check these. Sometimes it'll alert you that the Fibre Channel (FC) connection was lost. This is usually due to a bad cable, SFP, connector, or card, but is usually just a glitch that causes a SAN dropout on the client side. If these are happening constantly it's worth checking out. The errors are pretty well described, if you see something that seems scary reach out to Symply's support. If you address the error make sure to clear it so we know when new errors appear. 


### Adding a computer to the network

If you want to add a new SAN client to the network, navigate to _Provisioning_ then click the button that says _Host_. This will show all the available hosts. You'll see a list of hosts like:

* MDC1
*	MDC2
*	MDC2iscsi
*	CaptureStation3
*	CaptureStation2
*	CaptureStation01
*	PresSuite
*	CaptureStation04
*	CaptureStation06

If you add a host please try to keep the naming convention consistent. To add a host press the _Create_ button and select _Manually Create_. You'll need to know the WWPN/IQN of the Fibre Card installed on the computer, which you can get from the _Fibre Channel_ preference pane. You shouldn't have to do this without IT help, but it's good to know where this info is.
