# Flipper Zero Evil Portal 😼

An evil captive portal Wi-Fi access point using the Flipper Zero and Wi-Fi dev board

## About

**This repo is a fork from the original work to add [Electronic Cats Marauder](https://github.com/ElectronicCats/flipper-shields/wiki/2.-Flipper-Add%E2%80%90On:-Marauder-%E2%80%90-Marauder-Spoof) support**

This project will turn your Electronic Cats Marauder/Marauder Spoof Add-On into an open access point. When users try to connect to this access point they will be served a fake login screen. User credentials are sent to the Flipper and logged on the SD card.

## Portals

Please check out the [community portals folder](https://github.com/bigbrodude6119/flipper-zero-evil-portal/tree/main/portals) for more portals. 

Users, remember to rename the new portal as `index.html` when you drag it on the flipper SD card.

## Disclaimer

Use this for educational purposes only. We are not responsible for any damage caused.

# Getting Started

Download from the [releases section](https://github.com/ElectronicCats/flipper-zero-evil-portal/releases) and extract the `evil_portal_sd_folder.zip` folder. This .zip file contains a `evil_portal` folder.

Put the `evil_portal` folder into the `apps_data` folder on your SD card.
This is an example of your Flipper SD card if done correctly.

```
apps_data/
  evil_portal/
    ap.config.txt
    index.html
    logs/
      <empty>
```

You should be able to see the `[ESP32] Evil Portal` app on your flipper zero now.

# Installing/flashing the ESP32-S3 module on Marauder

>[!IMPORTANT]
> **Keeping the ESP32-S3 updated to the latest Portal firmware version ensures the correct functionality.**

As in other applications, the ESP32-S3 module is not the main MCU when using it with a flipper. This means that the firmware updates should be done using the USB-C port on the Add-On and not through the Flipper's USB.

Follow the next steps to flash the firmware:

1. Go to the [ESPWEBTOOL](https://esp.huhn.me/).

<p align="center">
	<img src="https://github.com/ElectronicCats/flipper-shields/assets/107638696/6893b5b0-5500-4891-af80-388cd1dd3b32" width=80%>
</p>

2. Attach the Marauder/ Marauder Spoof Add-On to the Flipper. 

3. Plug the USB cable into the USB-C port on the Add-On, enter the ESP32-S3 module in bootloader mode.
<p align="center">
	<img src="https://github.com/ElectronicCats/flipper-shields/assets/107638696/01a3a579-4aad-4cbe-b7d8-7e76d97bad2f" width=80%>
</p>

4. Click connect on ESPWEBTOOL. A pop-up menu will appear, select the correct board and port.

5. Use the following table to select the appropriate files and place them at the corresponding addresses.

Click on the register numbers to download!
|bin|Register| 
|--|--|
|Bootloader|[0x0](https://github.com/justcallmekoko/ESP32Marauder/raw/master/FlashFiles/FlipperZeroMultiBoardS3/esp32_marauder.ino.bootloader.bin)|
|Partitions|[0x8000](https://github.com/justcallmekoko/ESP32Marauder/raw/master/FlashFiles/FlipperZeroMultiBoardS3/esp32_marauder.ino.partitions.bin)|
|Boot App|[0xE000](https://github.com/justcallmekoko/ESP32Marauder/raw/master/FlashFiles/FlipperZeroMultiBoardS3/boot_app0.bin)|
|Firmware|[0x10000](https://github.com/ElectronicCats/flipper-zero-evil-portal/releases/download/v1.0/EvilPortal_ESPS3.ino.bin)|

6. Click on _PROGRAM_. A confirmation pop-up window will appear, click on _CONTINUE_.

7. The update will start immediately. **Do not disconnect the USB cable or detach the Add-On from the flipper while updating**.

8. Once the update has been finalized, press the reset button.

<p align="center">
	<img src="https://github.com/ElectronicCats/flipper-shields/assets/107638696/4fb77b3e-3f44-418b-bde2-f23eaa9fafca" width=80%>
</p>

Now you can unplug the USB cable and you are ready!

# Usage

Plug in the Marauder Add-On board to the flipper.

Open the app on the Flipper and press `Start portal` on the main menu. After a few seconds you should start to see logs coming in from your Wi-Fi dev board and the AP will start and the LED will turn green.

The AP will take the name that is in the `ap.config.txt` file located on your Flipper in the `apps_data/evil_portal/` folder.

When you connect to the AP a web page will open after a few seconds. This web page contains the HTML located in the `index.html` file located on your Flipper in the `apps_data/evil_portal/` folder.

You can stop the portal by pressing `Stop portal` on the main menu. The LED should turn blue.

You can manually save logs using the `Save logs` command. Logs will be stored in the `logs` folder that is in your `apps_data/evil_portal/` folder.

Logs will automatically be saved when exiting the app or when the current log reaches 4000 characters.

## Troubleshooting

- If you run into any issues make sure that you have the required files set up on the Flipper `apps_data` folder on the Flipper SD card.

- If the AP won't start or you have other issues try pressing reset on the Wi-Fi dev board, waiting a few seconds, and pressing `Start portal` on the main menu.

- It is important to give the dev board some time to load the html files from the Flipper.

- If you have the Marauder firmware on your dev board you **might** need to enable `Erase All Flash Before Sketch Upload` before flashing or follow the `Erasing firmware` instructions below. If you are using the web flasher there is an erase function on the website.

- If you see garbage characters in the AP name you will need to press the reset button on the board.

- Some users are reporting that the captive portal login does not open on some Android phones.

## Erasing firmware <a name="erasing-firmware"></a>

Assuming you have the Flipper Zero Wi-Fi Wrover Development Module (**ESP32-S2**):

1. Install [Python][link-python].
2. Open a command terminal as an administrator:
   - On Windows press ⊞Win+R, type "cmd", and press CTRL+SHIFT+ENTER.
3. In the terminal type the following to install [esptool][link-esptool] via Python package manager:
   ```
   pip install esptool
   ```
4. Install [setuptools][link-setuptools] dependencies:
   ```
   pip install setuptools
   ```
5. Enter the following command into your terminal, do not run it yet:
   ```
   python -m esptool --chip esp32s2 erase_flash
   ```
6. On your ESP32-S2 Wi-Fi module, hold the BOOT button.
7. Connect your ESP32-S2 to your computer, keep holding the BOOT button.
8. In your terminal press enter to run the command from step 5.
9. When successful you will get the message `Chip erase completed successfully in ___s` (time in seconds suffixed with "s").
10. Unplug/reset your board.

## License

Distributed under the MIT License. See `LICENSE.txt` for more information.

## Acknowledgments

I was only (the creator) able to create this using the following apps as examples

- [flipperzero-wifi-marauder](https://github.com/0xchocolate/flipperzero-wifi-marauder)
- [UART_Terminal](https://github.com/cool4uma/UART_Terminal)
- [flipper-zero-fap-boilerplate](https://github.com/leedave/flipper-zero-fap-boilerplate)
- [Create Captive Portal Using ESP32](https://iotespresso.com/create-captive-portal-using-esp32/)

## Contact me (the creator)

You can message me on my reddit account bigbrodude6119

<!-- LINKS -->

[link-arduino]: https://www.arduino.cc/en/software
[link-asynctcp]: https://github.com/me-no-dev/AsyncTCP
[link-espasyncwebserver]: https://github.com/me-no-dev/ESPAsyncWebServer
[link-esptool]: https://pypi.org/project/esptool/
[link-python]: https://www.python.org/downloads/
[link-setuptools]: https://pypi.org/project/setuptools/
