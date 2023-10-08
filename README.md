# DNDSync
This App was developed to enable synchronization between the smartphone and the **Galaxy Watch 4** of the
**Do Not Disturb** (*DND*) and **Bedtime mode** from [Digital Wellbeing](https://play.google.com/store/apps/details?id=com.google.android.apps.wellbeing&hl=en_US).
*DND* synchronization is only supported officially if using a Samsung phone with this smartwatch, Bedtime mode synchronization
is only made available for the newest Pixel Watch 2:
* With this repo you get ***both***!

If installed on phone and watch it enables either a 1-way sync or a 2-way sync of DND, depending on the preferences.
I also added the functionality to automatically toggle Bedtime Mode. Use case: At night I put my phone into DND and I want my watch to automatically enable Bedtime Mode.
This functionality is realized via modifying secure settings.


Majority of the credits goes to [@rhaeus](https://github.com/rhaeus) for the initial developing of this app
and to [@DreadedLama](https://github.com/DreadedLama) for the initial developing of a better bedtime mode implementation!

**Tested on Nothing Phone (1) (*Android 13*) paired with a Galaxy Watch 4 (*40mm*, *Wear OS 4.0*)**

## Setup

***Manual installation is required. The use of ADB is required. (*Don't worry, it's very easy!*)***

* Download the .apk files from the release section (`dndsync-mobile.apk` and `dndsync-wear.apk`)
* Be sure to enable notifications for Bedtime mode of the Digital Wellbeing app on your phone (*They are by default*)
    * This app knows that bedtime mode is activated when its notification pops up since there's no public API for the *Digital Wellbeing* app

### Phone

<img src="/images/mobile.png" width="300">

1. Install the app `dndsync_mobile.apk` on the phone (normally or via *adb*)
2. Open the App and grant the permission for DND Access by clicking on the menu entry *DND Permission*. This will open the permission screen.
    * This Permission is required so that the app can *read* and write DND state. Without this permission the sync will not work.
3. Go back on the app and check that `DND Permission` say **Granted** (you may need to tap on the menu entry for it to update)
4. Turn on the switch for the mode that you'd like to sync (*you can enable both, of course*):
    * With the **Sync DND state to watch** switch you can enable and disable the sync for *DND* mode.
      If enabled, a *DND* change on the phone will lead to *DND* change on the watch.
    * With the **Sync Bedtime mode to watch** switch you can enable and disable the sync for bedtime mode.
      If enabled, when *Bedtime mode* is *enabled/disabled/paused* on the phone, it will be *enabled/disabled/paused* on the watch

### Watch
<p float="left">
  <img src="/images/wear_1.png" width="200" />
  <img src="/images/wear_2.png" width="200" /> 
  <img src="/images/wear_3.png" width="200" />
  <img src="/images/wear_4.png" width="200" />
</p>

Setting up the watch is a bit more *tricky* since the watch OS lacks the permission screen for DND access,
but the permission needed can be **easily set via ADB***!

Note: This is only tested on my **Galaxy Watch 4** and it might not work on other devices!
1. Connect the watch to your computer via **adb** (watch and computer have to be in the *same network!*)
    * Enable Developer Options: Go to *Settings -> About watch -> Software -> tap the Software version 5 times -> developer mode is on (you can disable it in the same way)*
    * Enable *ADB debugging* and *Debug over WIFI* (in *Settings -> Developer Options*)
    * Note the watch IP address and port, something like `192.168.0.100:5555`
    * Connect to the watch with `adb connect 192.168.0.100:5555` (***insert your value!***)
2. Install the app `dndsync_wear.apk` on the watch
    * `adb install dndsync_wear.apk`
3. Grant permission for DND access (*This allows the app to listen to DND changes and changing the DND setting*)
    * `adb shell cmd notification allow_listener de.rhaeus.dndsync/de.rhaeus.dndsync.DNDNotificationService`  
4. Grant permission for Secure Setting access (*This allows the app to change BedTime mode setting*)
    * `adb shell pm grant de.rhaeus.dndsync android.permission.WRITE_SECURE_SETTINGS`
5. Open the app on the watch, scroll to the permission section and check if both `DND permission` 
   and `Secure Settings` Permission says ***Granted*** (you may need to tap on the menu entries for them to update)
6. ***IMPORTANT: Disable *ADB debugging* and *Debug over WIFI* after you are done because these options drain the battery!***
7. Turn on the switch for the mode that you'd like to sync (*you can enable all of them, of course*)
    * If you enable the setting _Sync DND_ in the App, a DND change on the watch will lead to a DND change on the phone
    * If you enable the setting _Bedtime Mode_ in the App, the watch will copy the bedtime mode status of the phone, it's a 1-way sync
    * If you enable the setting _Power Saver Mode_ in the App, the watch will turn on power saver mode whenever the _Bedtime Mode_ is synced from the phone
    * If you enable the setting _Vibration_ in the App the watch will vibrate whenever it receives a sync request from the phone

# Note
If you are unable to "Allow Notification Access" to the mobile app and it is faded, go the apps section in Settings, open DNDSync app, click 3 dots on top right and grant "Allow Restricted Settings" access.
Now you'll be able to grant the Notification access to mobile app.
