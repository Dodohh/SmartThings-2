# Instructions for setting up Powermax into SmartThings

This code has been written to interface an ESP8266 Wemos D1 R2 into SmartThings. The Arduino code is highly based on the PMax C++ library created by unknown people on https://www.domoticaforum.eu and then integrated onto a Wemos D1 R2 by irekz (https://github.com/irekzielinski/PowerMaxAlarm). I have then modified it and created some extra functions in order to interface with SmartThings.

Some important points to note:
1) The website created by the ESP8266 is currently not password protected (on my to do list eventually). I take no responsibility for your Powermax security!
2) It is currently a bit manual to get all devices set up, hence I strongly advise you make SmartThings and the Wemos have a fixed IP address in your router config.
3) I am using Arduino Studio 1.6.9 and version 2.3.0 of the ESP8266 board library. There is no reason I can think of, why it would not work with another version, but if you are having problems please ensure you use this version.
4) The motion sensors are a bit hard to work with as there is no notification that motion has stopped, hence without the SmartApp installed at the bottom of the instructions you will find the devices never change from 'Active'. I am currently implementing a solution on the Wemos.

The key steps to follow when setting up the integrations are as follows:
1) First set up Arduino Studio and add the Wemos D1 R2 (go to Tools menu, then Boards, then board manager and search for ESP8266)
2) Confirm that you now have the required ESP8266 libraries installed (if not you will need to add them from Sketch/Include Libraries/Manage Libraries)
3) Connect the Wemos D1 R2 to your computer and ensure the drivers are correct in order for it to show up in Arduino Studio
4) Add the PMAX library folder (4 files) to Arduino studio (the Libraries folder is normally in MyDocuments/Arduino/Libraries)
5) Create a sketch and call is whatever you like (mine is called 'PowerMaxEsp8266'). Copy the Arduino code in this folder into your sketch and update the settings at the top of the sketch for your personal WiFi and the IP address of SmartThings (under the heading //CHANGE THESE SETTINGS)
6) Ensure you select Wemos D1 R2 as the board, ensure the COM settings are correct. Then compile the code and upload to the Wemos
7) Connect the Wemos to the Powermax alarm. You need four connections: Power, Ground, RX and TX and can get further assistance on the setup from here - https://github.com/irekzielinski/PowerMaxAlarm. I use 12V since it is easy to get from inside the alarm, and plugs into the barrel connector quickly and easily. If using 3V3 from the alarm then this is fine and you can connect directly to the Wemos, but if using 5-24V then ensure you connect via the barrel connector.
8) Check the Wemos is connected to your WiFi by looking at your router and getting the IP address. Set the router to give a fixed IP address to the Wemos if possible
9) Check the Wemos is talking to the Powermax alarm by visiting the IP address of the Wemos and clicking 'Alarm status' under the JSON endpoint section. You should see the status at the top of the Status page saying disconnected, eventually it should say disarmed and then finally it will show the zones at the end of the page. The Wemos will auto enroll on Powermax Complete (can take 5minutes), but on other alarms you will need to force the enroll by going into the Powermax Installer Mode (again please allow 5 minute). If you are struggling to get this to work, then try resetting the Wemos by pressing the small button and then waiting again.
10) You should now be able to press Disarm/ArmHome/ArmAway on the Wemos webpage and you should hear your alarm respond!
11) Install the VisonicAlarmPanel device handler by copying in and pasting from Code. Ensure you press Save and Publish.
12) Modify the VisonicAlarmPanel device handler to add extra tiles for each zone (lines 81-124 currently has 5 zones added, either add extra zones and change the IDs or remove zones if they are not available in your system). Add the newly created tiles to the details section (lines 143-144), or remove the items for zones you just deleted.
13) Create a device for your Visonic Alarm and name it as you wish (I suggest 'Visonic Alarm Panel'). In the Device Network ID paste in the Wemos MAC address, this should be entered in capital letters and without colons (i.e. AABBCCDDEEFF)
14) Open the device on your phone and press 'Configure'. You should see the device populate information about status and zone names.
You can stop here if you want and you now have control of your Powermax Alarm from SmartThings, plus some status information. Alternatively continue to get separate zones in SmartThings (they will be easier to interface with things like CoRE)
15) Install the VisonicMultiZone device handler and the VisonicZoneManager smart app. Remember to Save and Publish.
16) Create a device for each zone that you have, and ensure it is setup to use the VisonicMultiZone device handler. Set the name to whatever you wish (I suggest 'Visonic Zone Name' where zone name is the name inside the Powermax). Set the device network ID to be 'visoniczoneX' (where X is the actual zone number in Powermax) - You can get this information from the Visonic Alarm Panel and the name must be 'visoniczoneX' (e.g. visoniczone1 or visoniczone4)
17) Now add the Smart App (you can do this through the App by going to the Automations tab and then selecting SmartApps at the top, followed by 'Add a SmartApp' and choose from 'My Apps' at the bottom.
18) Inside the SmartApp, first select the Alarm panel and then choose each of your devices/zones before giving it a name (I suggest 'Visonic Zone Manager') and pressing Save

You should finally have a fully linked Visonic Powermax Alarm inside SmartThings and have each of the zones separately broken out!! Woop!!