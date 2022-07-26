# **GARMIN-xDrip-Spike-Nightscout-Watchface**

xDrip-Spike-Spike-Nightscout-Watchface gets your CGM bloodglucose readings directly to your wrist! 

This watchface displays the time and date, your last blood glucose reading including a graph, the trend and age of the last reading, your daily steps including a step related progress bar, your actual heartrate (if enabled in the settings of your device), a notification count and alarm count..

Additionally if you have closed the loop with AndroidAPS it shows your loop status (IOB, TBR and COB).
Spike user can see IOB and COB, but unfortunately no basal is submitted. 

There are four phone synchronization possibilities: 
1. xDrip+ (Android only, no internet connection required)
1. Spike (iOS only, no internet connection required) 
1. Diabox (Android, iOS, no internet connection required)
1. Nightscout (cross-platform, internet connection required). 

------------------------
## **PLEASE READ THIS ADVISORY FIRST**
Never make a medical decision based on a reading that you see on this app e.g. your watch. Always perform a fingerstick blood glucose check first.

## **Contributing**
Please report bugs, errors and feature requests. Before doing so, please read the [Troubleshooting Q&A](#troubleshooting-qa).

------------------------
## **Watchface Settings**
You can change the settings in the Garmin Connect Mobile App or Garmin Express on a Windows/Mac. Available settings:

1. Choose your companion app: xDrip+, Spike or Nightscout (optional secured with an access-token) 
1. Adjust the lower and upper target of your bloodglucose readings (standard: 70-180 mg/dl / 3,9 mmol/l - 180 mg/dl / 10,0 mmol/l).
1. Change the delay of the request (standard: 15-30 seconds after the last reading) by adding or substracting some seconds if the timing does not work.
1. Eco Mode: If blood sugar is stable reduce value queries to at least 10 minutes. The word "eco" after the display of the delay indicates that the next value will not be read for about 10 minutes.
1. Low Power Mode: Displays a dark, reduced watchface with clock and CGM value when the watch is in low power mode (recommended for amoled displays).
1. Additional settings: Colorize bars and bg value, show notification count if any

------------------------
## **Setup**
### **Install Companion App**
This watchface can receive data from either xDrip+, Diabox or Spike (which are all apps on your phone) or from a Nightscout instance.

You need to install the xDrip+ (Android only) or Spike app (iOS only):

- Android: xDrip+ https://github.com/NightscoutFoundation/xDrip/releases or https://jamorham.github.io/#xdrip-plus
- Android: Diabox https://www.bubblan.org/
- iOS: https://spike-app.com
- Nightscout: https://github.com/nightscout/cgm-remote-monitor/releases or http://www.nightscout.info/

**You can not find these apps in app stores!**

See the following section on how to setup the individual applications.
### **xDrip+**
1. In xDrip+, enable the xDrip Web-Server (Settings -> Inter-App Settings -> enable "xDrip Web-Server", but *not* the "Open Web Server")
1. in the watchface settings select "xDrip+ (Android only)"

#### *Test xDrip+*
If you'd like to see, if xDrip+ is configured correctly, query the URL `http://127.0.0.1:17580/sgv.json?count=12` in your phone's webbrowser. If there is some text output with time stamps and glucose values, xDrip is set up correctly.

### **Spike**
1. In Spike, enable the Internal HTTP Server (Settings -> Integration -> Internal HTTP Server).
1. In the watchface settings select "Spike (iOS only)".

#### *Test Spike*
See [Text xDrip+](#test-xdrip), but change the URL to `http://127.0.0.1:1979/sgv.json?count=2`.

### **Diabox**
1. In Diabox, enable "Share data with smartwatches" (Diabox: Settings -> Integrations) 
1. In the watchface settings select "xDrip+ (Android only)".

#### *Test Diabox*
See [Test xDrip+](#test-xdrip).

### **Nightscout**
1. In the watchface settings select "Nightscout URL (mobile data connection required)"
1. Enter your nightscout URL, but without `https` or `/api/v1/...`, e.g. `yourapp.heroku.com`.

If your nightscout requires an api-password (i.e. is not readable to the whole internet), you need to generate an access token. If not, you can leave the `token` field empty.

**Security Advise:** For the sake of your very personal medical data, please do not expose your nightscout to the internet without a password protection!

3. To generate an access token, open your nightscout website, navigate to "Admin Tools" and click "Add new Subject". Supply a name (like `garmin`) and a role (the role must be at least `readable`!) and hit Save.
4. copy the access token (looks like `garmin-XXXXXXXXXXXX`) to the corresponding field inside the watchface settings of the Garmin Connect Mobile App or Garmin Express.

#### *Testing Nightscout*
See [Test xDrip+](#test-xdrip), but change the URL to
- `https://<YOURAPP.HEROKU.COM>/api/v1/entries/sgv.json?count=12` in case, you need no access token
- or `https://<YOURAPP.HEROKU.COM>/api/v1/entries/sgv.json?count=12&token=<ACCESS-TOKEN>` in case, you need an access token.

### **AndroidAPS**
If you like to see your loop status enable “xDrip+ Statusline (Watch)” in AndroidAPS.

------------------------
## **Troubleshooting Q/A**
*Q:* My watch does not display the glucose data instantly. What can I do? 
<br/>
*A:* Check the settings via the Garmin Connect Mobile App and wait at least 5 minutes. The Garmin SDK only allows data polling every 5 minutes, faster update rates are not possible.

*Q:* How do I get xDrip+ alarms and how can I snooze them?
<br />
*A:* Unfortunately Watchfaces cannot issue alarms automatically (sound, vibration, etc.). But it is possible to display and acknowledge/snooze the xDrip+ alarms on the watches. To do this, the xDrip+ app must be allowed to send messages in the Garmin Connect Mobile app: <br />
Settings > Notifications > App Notifications <br />
- Notification Access: enabled <br />
- xDrip+: enabled<br />
Another possibility is that a watch setting is working against it. Go to:<br / >
Garmin Connect Mobile App > Garmin Devices > Your device > Sounds & Alerts > Smart Notifications.<br />
Check if  the notifications under "General Use" and "During an Activity" are enabled (Show: all, Privacy: off, Alert: not "none").

*Q:* The watchfaces has a delay with respect to what's shown in xDrip+. What can I do?
<br/>
*A:* If you think the synchronisation between the app and xDrip+ isn't perfect adjust the delay in the settings.

*Q:* After some time, the blood glucose values are no longer updated ...
<br />
*A:* Often, energy-saving measures of the cell phone firmware lead to the fact that apps necessary for the query of blood glucose values are closed in the background. In particular, the Garmin Connect Mobile app and both Bluetooth apps (system apps) should not be restricted.
The website https://dontkillmyapp.com/ explains what to do for various cell phone types.

*Q:* The glucose data is not updated automatically on my **Fenix 6 & Enduro**
<br/>
*A:* Unfortunately, the firmware of the Fenix 6 and Enduro series has a bug that prevents updating the blood glucose data. This is not a problem with this watchface, there is nothing I can do about it:
https://forums.garmin.com/developer/connect-iq/i/bug-reports/fenix-6-with-background-app-watchface-doesn-t-update
<br/>**Starting from the public beta 20.82, this issue seems fixed!** The new firmware can be dowloaded [here](https://forums.garmin.com/outdoor-recreation/outdoor-recreation/f/fenix-6-series/290079/fenix-6-series---20-82-public-beta). Please check, if there is even newer software meanwhile.


*Q:* I get an "Error: -400".
<br/>
*A:* There is a communication error between the companion app (xDrip+, Diabox, Nightscout or Spike) and the Garmin Connect Mobile app:
INVALID_HTTP_BODY_IN_NETWORK_RESPONSE - Response body data is invalid for the request type.

## **Documented Error Codes**
See the error code table from the [Garin SDK Documentation](https://developer.garmin.com/connect-iq/api-docs/Toybox/Communications.html).

Some of the codes are handled explicitely:
| Error Code | Explanation | Possible Solution |
|------------|--------|-------------------|
| `Bluetooth!` | The Phone is not connected. | Reconnect your phone to the Garmin device |
| `Error: -1`<br/>`Bluetooth?` | `BLE_ERROR`: A generic BLE error has occurred. | Check the bluetooth connection settings |
| `Error: -2`<br/>`Bluetooth?` | `BLE_HOST_TIMEOUT`: We timed out waiting for a response from the host. | Check the bluetooth connection settings |
| `Error: -104`<br/>`Bluetooth?` | `BLE_CONNECTION_UNAVAILABLE`: No BLE connection is available. | Check the bluetooth connection settings |
| `Error: -300`<br/>`Settings?` | `NETWORK_REQUEST_TIMED_OUT`: Request timed out before a response was received | Check the settings of the watchface and your companion app. See [Setup](#setup) |
| `Error: -400` | `INVALID_HTTP_BODY_IN_NETWORK_RESPONSE`: Response body data is invalid for the request type. | Check the settings of the watchface and your companion app. See [Setup](#setup) |
| `Error: -401` | `UNAUTHORIZED`:  Unauthorized web access. | Most likely connected to Nightscout integration. Check the URL, the access token if the token has `readable` role. |
| `Error: -403`<br/>`Device Memory!` | `NETWORK_RESPONSE_OUT_OF_MEMORY`: Ran out of memory processing network response. | We messed something up. Please contact us! | 
| `Error: -404`<br/>`URL Settings?` | `PAGE_NOT_FOUND`: Check, if xDrip/Spike/Diabox are configured correctly, or if the nightscout URL and token are provided. | 

------------------------
## **Changelog**

V3.90 - Support for new devices (Forerunner 955 series, Edge 1040 series)
V3.81 - Format Spikes IOB without decimal digits
V3.80 - Spike App: Show IOB and COB (TBR isn't supported afaik)
V3.75 - Try to fix the problem of missing data if the watchface was not active for a while (code from Vaughanabe13)<br />
V3.70 - Swap Icons and Text for steps / stairs Up / heartrate, exchange some icons (@Trenar) and introduce icons for AAPS status (@swissalpine)<br />
V3.60 - Possibility to secure Nightscout with a readable token; alarm clock icon to show active alarms - Readme (all credits for these extensions to Trenar!))<br />
V3.56 - Fix for display error with double trend arrows (Thanks to Trenar!) <br />
V3.55 - New function and setting: Low power mode (recommended for devices with amoled display) <br />
V3.52 - Support for new devices <br />
V3.52 - Fix unhandled exception <br />
V3.51 - Try to fix crashes caused by unupdated settings file <br />
V3.50 - New Setting: Show notification count <br />
V3.48 - New Setting: Colorize progress bar <br />
V3.47 - Option to colorize BG value instead of the lines <br />
V3.46 - Small fixes <br />
V3.45 - Show actual heartrate if available, possibility to choose another color for inrange bg <br />
V3.40 - Support for new devices, small fixes <br />
V3.35 - Support for Enduro series <br />
V3.30 - Fix for Omnipod (AAPS only): Show TBR as an absolute value. <br />
V3.26 - Code clean up<br />
V3.25 - New setting: Disable data rotation and pin heartrate or steps <br />
V3.20 - Layout changes: Thanks for the critical feedback to V3.30 ... <br />
V3.14 - Fix 12 hour clock (0:00 to 12:00) <br />
V3.13 - Support for Descent MK2 <br />
V3.12 - Allow up to 120 sec delay <br />
V3.11 - Fix AAPS output if loop is deactivated in languages other than english/followers <br />
V3.10 - Bug fix: trend arrow doesn't change (mmol/l) <br />
V3.05 - Make the graph background black again! <br />
V3.01 - Fix broken Nightscout request <br />
V3.00 - Layout redesign to allow support of many new devices <br />
V2.23 - Fix division by zero if step goal is set to 0 <br />
V2.22 - Compile with SDK 3.1.8 in the hope of solving issues on individual devices (f. e. fr735xt) <br />
V2.21 - Added setting to change units (Set by app, mg/dl or mmol/l) <br />
V2.20 - Possibility to also query a Nightscout URL via the Internet <br />
V2.17 - Reduce calculations in the hope of saving energy; compiled with an older sdk (3.1.4 instead of 3.1.6) to reduce incompatibilities with non-up-to-date firmwares of some devices <br />
V2.16 - Fix NPE when xDrip+ is misconfigured <br />
V2.15 - Fix to maintain previous readings when switching screens <br />
V2.12 - Fix eco mode cannot be switched off <br />
V2.11 - Eco mode, fix loading previous values after screen change on some devices <br />
V2.04 - Code clean up <br />
V2.03 - Small bug fix <br />
V2.02 - Small bug fix <br />
V2.00 - Support for Spike app (iOS) <br />
[...] <br />
V1.00 - Initial release

## **About**
Thanks to Trenar (https://github.com/Trenar) for this Readme, the new alarm count function and some bugfixes<br />
Thanks to Vaughanabe13 (https://github.com/Vaughanabe13), who already showed me years ago how to restore a watchface with the last data

This Watchface uses the
<a target="_blank" href="https://icons8.com/icon/84881/heart">Heart</a>,
<a target="_blank" href="https://icons8.com/icon/5871/stairs-up">Stairs Up</a> (modified),
<a target="_blank" href="https://icons8.com/icon/22516/shoe-print">Shoe Print</a> (modified)
icons by <a target="_blank" href="https://icons8.com">Icons8</a>
