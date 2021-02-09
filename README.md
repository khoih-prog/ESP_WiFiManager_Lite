## ESP_WiFiManager_Lite (Light Weight Credentials / WiFi Manager for Generic WiFi (WiFiNINA, WiFi101, WiFiEsp, etc.)  modules/shields)

[![arduino-library-badge](https://www.ardu-badge.com/badge/ESP_WiFiManager_Lite.svg?)](https://www.ardu-badge.com/ESP_WiFiManager_Lite)
[![GitHub release](https://img.shields.io/github/release/khoih-prog/ESP_WiFiManager_Lite.svg)](https://github.com/khoih-prog/ESP_WiFiManager_Lite/releases)
[![GitHub](https://img.shields.io/github/license/mashape/apistatus.svg)](https://github.com/khoih-prog/ESP_WiFiManager_Lite/blob/main/LICENSE)
[![contributions welcome](https://img.shields.io/badge/contributions-welcome-brightgreen.svg?style=flat)](#Contributing)
[![GitHub issues](https://img.shields.io/github/issues/khoih-prog/ESP_WiFiManager_Lite.svg)](http://github.com/khoih-prog/ESP_WiFiManager_Lite/issues)

---
---

## Table of Contents

* [Why do we need this ESP_WiFiManager_Lite library](#why-do-we-need-this-esp_wifimanager_lite-library)
  * [Features](#features)
  * [Currently supported Boards](#currently-supported-boards)
* [Changelog](#changelog)
  * [Release v1.0.0](#release-v100)
* [Prerequisites](#prerequisites)
* [Installation](#installation)
  * [Use Arduino Library Manager](#use-arduino-library-manager)
  * [Manual Install](#manual-install)
  * [VS Code & PlatformIO](#vs-code--platformio)
* [Note for Platform IO using ESP32 LittleFS](#note-for-platform-io-using-esp32-littlefs)
* [HOWTO Use analogRead() with ESP32 running WiFi and/or BlueTooth (BT/BLE)](#howto-use-analogread-with-esp32-running-wifi-andor-bluetooth-btble)
  * [1. ESP32 has 2 ADCs, named ADC1 and ADC2](#1--esp32-has-2-adcs-named-adc1-and-adc2)
  * [2. ESP32 ADCs functions](#2-esp32-adcs-functions)
  * [3. ESP32 WiFi uses ADC2 for WiFi functions](#3-esp32-wifi-uses-adc2-for-wifi-functions)
* [How It Works](#how-it-works)
* [How to use](#how-to-use)
* [Examples](#examples)
  * [ 1. ESP_WiFi](examples/ESP_WiFi)
  * [ 2. ESP_WiFi_MQTT](examples/ESP_WiFi_MQTT)
* [So, how it works?](#so-how-it-works)
* [Important Notes](#important-notes)
* [How to use default Credentials and have them pre-loaded onto Config Portal](#how-to-use-default-credentials-and-have-them-pre-loaded-onto-config-portal)
  * [1. To always load Default Credentials and override Config Portal data](#1-to-always-load-default-credentials-and-override-config-portal-data)
  * [2. To load Default Credentials when there is no valid Credentials](#2-to-load-default-credentials-when-there-is-no-valid-credentials)
  * [3. Example of Default Credentials](#3-example-of-default-credentials)
* [How to add dynamic parameters from sketch](#how-to-add-dynamic-parameters-from-sketch)
* [Important Notes for using Dynamic Parameters' ids](#important-notes-for-using-dynamic-parameters-ids)
* [Example ESP_WiFi](#example-esp_wifi)
  * [1. File ESP_WiFi.ino](#1-file-esp_wifiino)
  * [2. File defines.h](#2-file-definesh)
  * [3. File Credentials.h](#3-file-credentialsh)
  * [4. File dynamicParams.h](#4-file-dynamicparamsh)
* [Debug Termimal Output Samples](#debug-terminal-output-samples)
  * [1. ESP_WiFi on ESP32_DEV](#1-esp_wifi-on-esp32_dev)
    * [1.1 MRD/DRD => Open Config Portal](#11-mrddrd--open-config-portal)
    * [1.2 Got valid Credentials from Config Portal then connected to WiFi](#12-got-valid-credentials-from-config-portal-then-connected-to-wifi)
    * [1.3 Lost a WiFi and autoconnect to another WiFi AP](#13-lost-a-wifi-and-autoconnect-to-another-wifi-ap)
  * [2. ESP_WiFi_MQTT on ESP8266_NODEMCU](#2-esp_wifi_mqtt-on-esp8266_nodemcu)
    * [2.1 No Config Data => Open Config Portal](#21-no-config-data--open-config-portal)
    * [2.2 Got valid Credentials from Config Portal then connected to WiFi](#22-got-valid-credentials-from-config-portal-then-connected-to-wifi)
    * [2.3 Lost a WiFi and autoconnect to another WiFi AP](#23-lost-a-wifi-and-autoconnect-to-another-wifi-ap)
* [Debug](#debug)
* [Troubleshooting](#troubleshooting)
* [Releases](#releases)
* [Issues](#issues)
* [TO DO](#to-do)
* [DONE](#done)
* [Contributions and Thanks](#contributions-and-thanks)
* [Contributing](#contributing)
* [License](#license)
* [Copyright](#copyright)

---
---

### Why do we need this [ESP_WiFiManager_Lite library](https://github.com/khoih-prog/ESP_WiFiManager_Lite)

#### Features

If you have used one of the full-fledge WiFiManagers such as :

1. [`Tzapu WiFiManager`](https://github.com/tzapu/WiFiManager)
2. [`Ken Taylor WiFiManager`](https://github.com/kentaylor/WiFiManager)
3. [`Khoi Hoang ESP_WiFiManager`](https://github.com/khoih-prog/ESP_WiFiManager)

and have to write **complicated callback functions** to save custom parameters in SPIFFS/LittleFS/EEPROM, you'd appreciate the simplicity of this Light-Weight Credentials / WiFiManager.

This is a Credentials / WiFi Connection Manager for ESP32 and ESP8266 boards, permitting the addition of custom parameters to be configured in Config Portal. The parameters then will be saved automatically, **without the complicated callback functions** to handle data saving / retrieving.

You can also specify DHCP HostName, static AP and STA IP. Use much less memory compared to full-fledge WiFiManager. Config Portal will be auto-adjusted to match the number of dynamic custom parameters. Credentials are saved in LittleFS, SPIFFS or EEPROM.

The web configuration portal, served from the `ESP32/ESP8266 WiFi` is operating as an access point (AP) with configurable static IP address or use default IP Address of 192.168.4.1

New recent features:

- **MultiWiFi** feature for configuring/auto(re)connecting **ESP32/ESP8266 WiFi** to the available MultiWiFi APs at runtime.
- **Multi/DoubleDetectDetector** feature to force Config Portal when multi/double reset is detected within predetermined time, default 10s.
- **Powerful-yet-simple-to-use feature to enable adding dynamic custom parameters** from sketch and input using the same Config Portal. Config Portal will be auto-adjusted to match the number of dynamic parameters.
- Optional default **Credentials as well as Dynamic parameters to be optionally autoloaded into Config Portal** to use or change instead of manually input.
- Dynamic custom parameters to be saved **automatically in non-volatile memory, such as LittleFS, SPIFFS or EEPROM.**.
- Configurable **Config Portal Title** to be either BoardName or default undistinguishable names.
- Examples are designed to separate Credentials / Defines / Dynamic Params / Code so that you can change Credentials / Dynamic Params quickly for each device.
- Control Config Portal from software or Virtual Switches
- To permit autoreset after configurable timeout if DRD/MRD or non-persistent forced-CP
- Use new ESP32 LittleFS features


#### Currently supported Boards

This [**ESP_WiFiManager_Lite** library](https://github.com/khoih-prog/ESP_WiFiManager_Lite) currently supports these following boards:

 1. **ESP32**
 2. **ESP8266**

---
---

## Changelog

### Release v1.0.0

1. Initial release to support ESP32 and ESP8266.

---
---

## Prerequisites

 1. [`Arduino IDE 1.8.13+` for Arduino](https://www.arduino.cc/en/Main/Software)
 2. [`ESP8266 Core 2.7.4+`](https://github.com/esp8266/Arduino) for ESP8266-based boards.
 3. [`ESP32 Core 1.0.4+`](https://github.com/espressif/arduino-esp32) for ESP32-based boards
 4. [`ESP_DoubleResetDetector v1.1.1+`](https://github.com/khoih-prog/ESP_DoubleResetDetector) if using DRD feature. To install, check [![arduino-library-badge](https://www.ardu-badge.com/badge/ESP_DoubleResetDetector.svg?)](https://www.ardu-badge.com/ESP_DoubleResetDetector).
 5. [`ESP_MultiResetDetector v1.1.1+`](https://github.com/khoih-prog/ESP_MultiResetDetector) if using MRD feature. To install, check [![arduino-library-badge](https://www.ardu-badge.com/badge/ESP_MultiResetDetector.svg?)](https://www.ardu-badge.com/ESP_MultiResetDetector).
 6. [`LittleFS_esp32 v1.0.5+`](https://github.com/lorol/LITTLEFS) for ESP32-based boards using LittleFS.

---

## Installation

### Use Arduino Library Manager

The best and easiest way is to use `Arduino Library Manager`. Search for [**ESP_WiFiManager_Lite**](https://github.com/khoih-prog/ESP_WiFiManager_Lite), then select / install the latest version.
You can also use this link [![arduino-library-badge](https://www.ardu-badge.com/badge/ESP_WiFiManager_Lite.svg?)](https://www.ardu-badge.com/ESP_WiFiManager_Lite) for more detailed instructions.

### Manual Install

1. Navigate to [**ESP_WiFiManager_Lite**](https://github.com/khoih-prog/ESP_WiFiManager_Lite) page.
2. Download the latest release `ESP_WiFiManager_Lite-main.zip`.
3. Extract the zip file to `ESP_WiFiManager_Lite-main` directory 
4. Copy the whole 
  - `ESP_WiFiManager_Lite-main` folder to Arduino libraries' directory such as `~/Arduino/libraries/`.

### VS Code & PlatformIO:

1. Install [VS Code](https://code.visualstudio.com/)
2. Install [PlatformIO](https://platformio.org/platformio-ide)
3. Install [**ESP_WiFiManager_Lite** library](https://platformio.org/lib/show/xxxxx/ESP_WiFiManager_Lite) by using [Library Manager](https://platformio.org/lib/show/xxxxx/ESP_WiFiManager_Lite/installation). Search for **ESP_WiFiManager_Lite** in [Platform.io Author's Libraries](https://platformio.org/lib/search?query=author:%22Khoi%20Hoang%22)
4. Use included [platformio.ini](platformio/platformio.ini) file from examples to ensure that all dependent libraries will installed automatically. Please visit documentation for the other options and examples at [Project Configuration File](https://docs.platformio.org/page/projectconf.html)

---
---

### Note for Platform IO using ESP32 LittleFS

In Platform IO, to fix the error when using [`LittleFS_esp32 v1.0`](https://github.com/lorol/LITTLEFS) for ESP32-based boards with ESP32 core v1.0.4- (ESP-IDF v3.2-), uncomment the following line

from

```
//#define CONFIG_LITTLEFS_FOR_IDF_3_2   /* For old IDF - like in release 1.0.4 */
```

to

```
#define CONFIG_LITTLEFS_FOR_IDF_3_2   /* For old IDF - like in release 1.0.4 */
```

It's advisable to use the latest [`LittleFS_esp32 v1.0.5+`](https://github.com/lorol/LITTLEFS) to avoid the issue.

Thanks to [Roshan](https://github.com/solroshan) to report the issue in [Error esp_littlefs.c 'utime_p'](https://github.com/khoih-prog/ESPAsync_WiFiManager/issues/28) 

---
---

### HOWTO Use analogRead() with ESP32 running WiFi and/or BlueTooth (BT/BLE)

Please have a look at [**ESP_WiFiManager Issue 39: Not able to read analog port when using the autoconnect example**](https://github.com/khoih-prog/ESP_WiFiManager/issues/39) to have more detailed description and solution of the issue.

#### 1.  ESP32 has 2 ADCs, named ADC1 and ADC2

#### 2. ESP32 ADCs functions

- ADC1 controls ADC function for pins **GPIO32-GPIO39**
- ADC2 controls ADC function for pins **GPIO0, 2, 4, 12-15, 25-27**

#### 3.. ESP32 WiFi uses ADC2 for WiFi functions

Look in file [**adc_common.c**](https://github.com/espressif/esp-idf/blob/master/components/driver/adc_common.c#L61)

> In ADC2, there're two locks used for different cases:
> 1. lock shared with app and Wi-Fi:
>    ESP32:
>         When Wi-Fi using the ADC2, we assume it will never stop, so app checks the lock and returns immediately if failed.
>    ESP32S2:
>         The controller's control over the ADC is determined by the arbiter. There is no need to control by lock.
> 
> 2. lock shared between tasks:
>    when several tasks sharing the ADC2, we want to guarantee
>    all the requests will be handled.
>    Since conversions are short (about 31us), app returns the lock very soon,
>    we use a spinlock to stand there waiting to do conversions one by one.
> 
> adc2_spinlock should be acquired first, then adc2_wifi_lock or rtc_spinlock.


- In order to use ADC2 for other functions, we have to **acquire complicated firmware locks and very difficult to do**
- So, it's not advisable to use ADC2 with WiFi/BlueTooth (BT/BLE).
- Use ADC1, and pins GPIO32-GPIO39
- If somehow it's a must to use those pins serviced by ADC2 (**GPIO0, 2, 4, 12, 13, 14, 15, 25, 26 and 27**), use the **fix mentioned at the end** of [**ESP_WiFiManager Issue 39: Not able to read analog port when using the autoconnect example**](https://github.com/khoih-prog/ESP_WiFiManager/issues/39) to work with ESP32 WiFi/BlueTooth (BT/BLE).


---
---

## How It Works

- The [**ESP_WiFi**](examples/ESP_WiFi) example shows how it works and should be used as the basis for a sketch that uses this library.
- The concept of [**ESP_WiFi**](examples/ESP_WiFi) is that a new `ESP32/ESP8266 WiFi` will start a WiFi configuration portal when powered up, but has no valid stored Credentials or can't connect to WiFi APs after a pre-determined time.
- There are 6 more custom parameters added in the sketch which you can use in your program later. In the example, they are: 2 sets of Blynk Servers and Tokens, Blynk Port and MQTT Server.
- Using any WiFi enabled device with a browser (computer, phone, tablet) connect to the newly created AP and type in the configurable AP IP address (default 192.168.4.1). The Config Portal AP channel (default 10) is also configurable to avoid conflict with other APs.
- The Config Portal is **auto-adjusted** to fix the 4 static parameters (WiFi SSIDs/PWDs) as well as 6 more dynamic custom parameters.
- After the custom data entered, and **Save** button pressed, the configuration data will be saved in host's non-volatile memory, then the board reboots.
- If there is valid stored Credentials, it'll go directly to connect to one of the **MultiWiFi APs** without starting / using the Config Portal.
- `ESP32/ESP8266 WiFi` will try to connect. If successful, the dynamic DHCP and/or configured static IP address will be displayed in the configuration portal. 
- The `ESP32/ESP8266 WiFi` Config Portal network and Web Server will shutdown to return control to the sketch code.
- In the operation, if the current WiFi connection is lost because of any reason, the system will **auto(Re)connect** to the remaining WiFi AP.
- **If system can't connect to any of the 2 WiFi APs, the Config Portal will start**, after some pre-determined time, to permit user to update the Credentials.

---

### How to use

- Include in your sketch

```cpp
// Must be before #include <ESP_WiFiManager_Lite.h>
#include <ESP_WiFiManager_Lite.h>

ESP_WiFiManager_Lite* ESP_WiFiManager;
```

- To add custom parameters, just add

```
#include "defines.h"

// USE_DYNAMIC_PARAMETERS defined in defined.h

/////////////// Start dynamic Credentials ///////////////

//Defined in <WiFiManager_Generic_Lite.h>
/**************************************
  #define MAX_ID_LEN                5
  #define MAX_DISPLAY_NAME_LEN      16

  typedef struct
  {
  char id             [MAX_ID_LEN + 1];
  char displayName    [MAX_DISPLAY_NAME_LEN + 1];
  char *pdata;
  uint8_t maxlen;
  } MenuItem;
**************************************/

#if USE_DYNAMIC_PARAMETERS

#define MAX_BLYNK_SERVER_LEN      34
#define MAX_BLYNK_TOKEN_LEN       34

char Blynk_Server1 [MAX_BLYNK_SERVER_LEN + 1]  = "account.duckdns.org";
char Blynk_Token1  [MAX_BLYNK_TOKEN_LEN + 1]   = "token1";

char Blynk_Server2 [MAX_BLYNK_SERVER_LEN + 1]  = "account.ddns.net";
char Blynk_Token2  [MAX_BLYNK_TOKEN_LEN + 1]   = "token2";

#define MAX_BLYNK_PORT_LEN        6
char Blynk_Port   [MAX_BLYNK_PORT_LEN + 1]  = "8080";

#define MAX_MQTT_SERVER_LEN      34
char MQTT_Server  [MAX_MQTT_SERVER_LEN + 1]   = "mqtt.duckdns.org";

MenuItem myMenuItems [] =
{
  { "sv1", "Blynk Server1", Blynk_Server1,  MAX_BLYNK_SERVER_LEN },
  { "tk1", "Token1",        Blynk_Token1,   MAX_BLYNK_TOKEN_LEN },
  { "sv2", "Blynk Server2", Blynk_Server2,  MAX_BLYNK_SERVER_LEN },
  { "tk2", "Token2",        Blynk_Token2,   MAX_BLYNK_TOKEN_LEN },
  { "prt", "Port",          Blynk_Port,     MAX_BLYNK_PORT_LEN },
  { "mqt", "MQTT Server",   MQTT_Server,    MAX_MQTT_SERVER_LEN },
};

uint16_t NUM_MENU_ITEMS = sizeof(myMenuItems) / sizeof(MenuItem);  //MenuItemSize;

#else

MenuItem myMenuItems [] = {};

uint16_t NUM_MENU_ITEMS = 0;

#endif    //USE_DYNAMIC_PARAMETERS

```

- If you don't need to add dynamic parameters, use the following in sketch

```
#define USE_DYNAMIC_PARAMETERS      false
```

- When you want to open a config portal, just add

```cpp
ESP_WiFiManager = new ESP_WiFiManager_Lite();
ESP_WiFiManager->begin();
```

- To not use default AP WiFi Channel 10 to avoid conflict with other WiFi APs, call 

```cpp
ESP_WiFiManager->setConfigPortalChannel(newChannel);
```

- To use different static AP IP (not use default `192.168.4.1`), call

```cpp
ESP_WiFiManager->setConfigPortalIP(IPAddress(xxx,xxx,xxx,xxx));
```

- To set custom DHCP HostName, cal
 
```
  // Set customized DHCP HostName
  ESP_WiFiManager->begin("SAMD_ABCDEF");
```
 
or just use the default Hostname, for example "SAMD_XXXXXX" for SAMD

```
  //Or use default Hostname "WIFI_GENERIC_XXXXXX"
  //ESP_WiFiManager->begin();
```

While in AP mode, connect to it using its `SSID` (WIFI_GENERIC_XXXXXX) / `Password` ("MyWIFI_GENERIC_XXXXXX"), then open a browser to the Portal AP IP, default `192.168.4.1`, configure wifi then click **Save**. The Credentials / WiFi connection information will be saved in non-volatile memory. It will then autoconnect.


Once Credentials / WiFi network information is saved in the host non-volatile memory, it will try to autoconnect to WiFi every time it is started, without requiring any function calls in the sketch.

---
---

### Examples

 1. [ESP_WiFi](examples/ESP_WiFi)
 2. [ESP_WiFi_MQTT](examples/ESP_WiFi_MQTT)

---
---

## So, how it works?

In `Configuration Portal Mode`, it starts an AP called `ESP_WXXXXXX`. Connect to it using the `configurable password` you can define in the code. For example, `MyESP_WXXXXXX` (see examples):

After you connected, please, go to http://192.168.4.1 or newly configured AP IP, you'll see this `Main` page:

<p align="center">
    <img src="https://github.com/khoih-prog/ESP_WiFiManager_Lite/blob/main/pics/Main.png">
</p>

Enter your credentials, 

<p align="center">
    <img src="https://github.com/khoih-prog/ESP_WiFiManager_Lite/blob/main/pics/Input.png">
</p>

then click `Save`. 

<p align="center">
    <img src="https://github.com/khoih-prog/ESP_WiFiManager_Lite/blob/main/pics/Save.png">
</p>

The WiFi Credentials will be saved and the board connect to the selected WiFi AP.

If you're already connected to a listed WiFi AP and don't want to change anything, just select `Exit` from the `Main` page to reboot the board and connect to the previously-stored AP. The WiFi Credentials are still intact.

---

### Important Notes

1. Now you can use special chars such as **~, !, @, #, $, %, ^, &, _, -, space,etc.** thanks to [brondolin](https://github.com/brondolin) to provide the amazing fix in [**Blynk_WM**](https://github.com/khoih-prog/Blynk_WM) to permit input special chars such as **%** and **#** into data fields. See [Issue 3](https://github.com/khoih-prog/Blynk_WM/issues/3).
2. The SSIDs, Passwords must be input (or to make them different from **blank**). Otherwise, the Config Portal will re-open until those fields have been changed. If you don't need any field, just input anything or use duplicated data from similar field.
3. WiFi password max length now is 63 chars according to WPA2 standard.

---

### How to use default Credentials and have them pre-loaded onto Config Portal

See this example and modify as necessary

#### 1. To always load [Default Credentials](examples//Credentials.h) and override Config Portal data

```
// Used mostly for development and debugging. FORCES default values to be loaded each run.
// Config Portal data input will be ignored and overridden by DEFAULT_CONFIG_DATA
bool LOAD_DEFAULT_CONFIG_DATA = true;
```

#### 2. To load [Default Credentials](examples//Credentials.h) when there is no valid Credentials.

Config Portal data input will be override DEFAULT_CONFIG_DATA

```
// Used mostly once debugged. Assumes good data already saved in device.
// Config Portal data input will be override DEFAULT_CONFIG_DATA
bool LOAD_DEFAULT_CONFIG_DATA = false;
```

#### 3. Example of [Default Credentials](examples/ESP_WiFi/Credentials.h)

```cpp
/// Start Default Config Data //////////////////

/*
#define SSID_MAX_LEN      32
//From v1.0.3, WPA2 passwords can be up to 63 characters long.
#define PASS_MAX_LEN      64

typedef struct
{
  char wifi_ssid[SSID_MAX_LEN];
  char wifi_pw  [PASS_MAX_LEN];
}  WiFi_Credentials;

#define NUM_WIFI_CREDENTIALS      2

// Configurable items besides fixed Header, just add board_name 
#define NUM_CONFIGURABLE_ITEMS    ( ( 2 * NUM_WIFI_CREDENTIALS ) + 1 )
////////////////

typedef struct Configuration
{
  char header         [16];
  WiFi_Credentials  WiFi_Creds  [NUM_WIFI_CREDENTIALS];
  char board_name     [24];
  int  checkSum;
} ESP_WM_LITE_Configuration;
*/

#define TO_LOAD_DEFAULT_CONFIG_DATA      false

#if TO_LOAD_DEFAULT_CONFIG_DATA

// This feature is primarily used in development to force a known set of values as Config Data
// It will NOT force the Config Portal to activate. Use DRD or erase Config Data with ESP_WiFiManager.clearConfigData()

// Used mostly for development and debugging. FORCES default values to be loaded each run.
// Config Portal data input will be ignored and overridden by DEFAULT_CONFIG_DATA
//bool LOAD_DEFAULT_CONFIG_DATA = true;

// Used mostly once debugged. Assumes good data already saved in device.
// Config Portal data input will be override DEFAULT_CONFIG_DATA
bool LOAD_DEFAULT_CONFIG_DATA = false;


ESP_WM_LITE_Configuration defaultConfig =
{
  //char header[16], dummy, not used
#if ESP8266 
  "ESP8266",
#else
  "ESP32",
#endif

  // WiFi_Credentials  WiFi_Creds  [NUM_WIFI_CREDENTIALS];
  // WiFi_Credentials.wifi_ssid and WiFi_Credentials.wifi_pw
  "SSID1",  "password1",
  "SSID2",  "password2",
  //char board_name     [24];

#if ESP8266 
  "ESP8266-Control",
#else
  "ESP32-Control",
#endif

  // terminate the list
  //int  checkSum, dummy, not used
  0
  /////////// End Default Config Data /////////////
};

#else

bool LOAD_DEFAULT_CONFIG_DATA = false;

ESP_WM_LITE_Configuration defaultConfig;

#endif    // TO_LOAD_DEFAULT_CONFIG_DATA

/////////// End Default Config Data /////////////
```

### How to add dynamic parameters from sketch

Example of [Default dynamicParams](examples/ESP_WiFi/dynamicParams.h)

- To add custom parameters, just modify the example below

```
#include "defines.h"

// USE_DYNAMIC_PARAMETERS defined in defined.h

/////////////// Start dynamic Credentials ///////////////

//Defined in <WiFiManager_Generic_Lite.h>
/**************************************
  #define MAX_ID_LEN                5
  #define MAX_DISPLAY_NAME_LEN      16

  typedef struct
  {
  char id             [MAX_ID_LEN + 1];
  char displayName    [MAX_DISPLAY_NAME_LEN + 1];
  char *pdata;
  uint8_t maxlen;
  } MenuItem;
**************************************/

#if USE_DYNAMIC_PARAMETERS

#define MAX_BLYNK_SERVER_LEN      34
#define MAX_BLYNK_TOKEN_LEN       34

char Blynk_Server1 [MAX_BLYNK_SERVER_LEN + 1]  = "account.duckdns.org";
char Blynk_Token1  [MAX_BLYNK_TOKEN_LEN + 1]   = "token1";

char Blynk_Server2 [MAX_BLYNK_SERVER_LEN + 1]  = "account.ddns.net";
char Blynk_Token2  [MAX_BLYNK_TOKEN_LEN + 1]   = "token2";

#define MAX_BLYNK_PORT_LEN        6
char Blynk_Port   [MAX_BLYNK_PORT_LEN + 1]  = "8080";

#define MAX_MQTT_SERVER_LEN      34
char MQTT_Server  [MAX_MQTT_SERVER_LEN + 1]   = "mqtt.duckdns.org";

MenuItem myMenuItems [] =
{
  { "sv1", "Blynk Server1", Blynk_Server1,  MAX_BLYNK_SERVER_LEN },
  { "tk1", "Token1",        Blynk_Token1,   MAX_BLYNK_TOKEN_LEN },
  { "sv2", "Blynk Server2", Blynk_Server2,  MAX_BLYNK_SERVER_LEN },
  { "tk2", "Token2",        Blynk_Token2,   MAX_BLYNK_TOKEN_LEN },
  { "prt", "Port",          Blynk_Port,     MAX_BLYNK_PORT_LEN },
  { "mqt", "MQTT Server",   MQTT_Server,    MAX_MQTT_SERVER_LEN },
};

uint16_t NUM_MENU_ITEMS = sizeof(myMenuItems) / sizeof(MenuItem);  //MenuItemSize;

#else

MenuItem myMenuItems [] = {};

uint16_t NUM_MENU_ITEMS = 0;

#endif    //USE_DYNAMIC_PARAMETERS

```
- If you don't need to add dynamic parameters, use the following in sketch

```
#define USE_DYNAMIC_PARAMETERS     false
```

or

```
/////////////// Start dynamic Credentials ///////////////

MenuItem myMenuItems [] = {};

uint16_t NUM_MENU_ITEMS = 0;
/////// // End dynamic Credentials ///////////

```
---

### Important Notes for using Dynamic Parameters' ids

1. These ids (such as "mqtt" in example) must be **unique**.

Please be noted that the following **reserved names are already used in library**:

```
"id"    for WiFi SSID
"pw"    for WiFi PW
"id1"   for WiFi1 SSID
"pw1"   for WiFi1 PW
"nm"    for Board Name
```
---
---

### Example [ESP_WiFi](examples/ESP_WiFi)

Please take a look at other examples, as well.

#### 1. File [ESP_WiFi.ino](examples/ESP_WiFi/ESP_WiFi.ino)

```cpp
#include "defines.h"
#include "Credentials.h"
#include "dynamicParams.h"

void heartBeatPrint()
{
  static int num = 1;

  if (WiFi.status() == WL_CONNECTED)
    Serial.print(F("H"));        // H means connected to WiFi
  else
    Serial.print(F("F"));        // F means not connected to WiFi

  if (num == 80)
  {
    Serial.println();
    num = 1;
  }
  else if (num++ % 10 == 0)
  {
    Serial.print(F(" "));
  }
}

void check_status()
{
  static unsigned long checkstatus_timeout = 0;

  //KH
#define HEARTBEAT_INTERVAL    20000L
  // Print hearbeat every HEARTBEAT_INTERVAL (20) seconds.
  if ((millis() > checkstatus_timeout) || (checkstatus_timeout == 0))
  {
    heartBeatPrint();
    checkstatus_timeout = millis() + HEARTBEAT_INTERVAL;
  }
}

ESP_WiFiManager_Lite* ESP_WiFiManager;

void setup()
{
  // Debug console
  Serial.begin(115200);
  while (!Serial);

  delay(200);

  Serial.print(F("\nStarting ESP_WiFi using ")); Serial.print(FS_Name);
  Serial.print(F(" on ")); Serial.println(ARDUINO_BOARD);
  Serial.println(ESP_WIFI_MANAGER_LITE_VERSION);

#if USING_MRD  
  Serial.println(ESP_MULTI_RESET_DETECTOR_VERSION);
#else
  Serial.println(ESP_DOUBLE_RESET_DETECTOR_VERSION);
#endif

  ESP_WiFiManager = new ESP_WiFiManager_Lite();

  // Optional to change default AP IP(192.168.4.1) and channel(10)
  //ESP_WiFiManager->setConfigPortalIP(IPAddress(192, 168, 120, 1));
  //ESP_WiFiManager->setConfigPortalChannel(1);

  // Set customized DHCP HostName
  ESP_WiFiManager->begin(HOST_NAME);
  //Or use default Hostname "NRF52-WIFI-XXXXXX"
  //ESP_WiFiManager->begin();
}

#if USE_DYNAMIC_PARAMETERS
void displayCredentials()
{
  Serial.println(F("\nYour stored Credentials :"));

  for (uint16_t i = 0; i < NUM_MENU_ITEMS; i++)
  {
    Serial.print(myMenuItems[i].displayName);
    Serial.print(F(" = "));
    Serial.println(myMenuItems[i].pdata);
  }
}

void displayCredentialsInLoop()
{
  static bool displayedCredentials = false;

  if (!displayedCredentials)
  {
    for (int i = 0; i < NUM_MENU_ITEMS; i++)
    {
      if (!strlen(myMenuItems[i].pdata))
      {
        break;
      }

      if ( i == (NUM_MENU_ITEMS - 1) )
      {
        displayedCredentials = true;
        displayCredentials();
      }
    }
  }
}

#endif

void loop()
{
  ESP_WiFiManager->run();
  check_status();

#if USE_DYNAMIC_PARAMETERS
  displayCredentialsInLoop();
#endif
}
```
---

#### 2. File [defines.h](examples/ESP_WiFi/defines.h)

```cpp
#ifndef defines_h
#define defines_h

#if !( ESP8266 || ESP32)
  #error This code is intended to run only on the ESP8266/ESP32 boards ! Please check your Tools->Board setting.
#endif

/* Comment this out to disable prints and save space */
#define ESP_WM_LITE_DEBUG_OUTPUT      Serial

#define _ESP_WM_LITE_LOGLEVEL_        3

#define USING_MRD                     true

#if USING_MRD
  #define MULTIRESETDETECTOR_DEBUG      true
  
  // Number of seconds after reset during which a
  // subseqent reset will be considered a double reset.
  #define MRD_TIMEOUT                   10
  
  // RTC Memory Address for the DoubleResetDetector to use
  #define MRD_ADDRESS                   0
  #warning Using MULTI_RESETDETECTOR
#else
  #define DOUBLERESETDETECTOR_DEBUG     true
  
  // Number of seconds after reset during which a
  // subseqent reset will be considered a double reset.
  #define DRD_TIMEOUT                   10
  
  // RTC Memory Address for the DoubleResetDetector to use
  #define DRD_ADDRESS                   0
  #warning Using DOUBLE_RESETDETECTOR
#endif

/////////////////////////////////////////////

// LittleFS has higher priority than SPIFFS
  #define USE_LITTLEFS    true
  #define USE_SPIFFS      false

/////////////////////////////////////////////

// Force some params
#define TIMEOUT_RECONNECT_WIFI                    10000L
#define RESET_IF_CONFIG_TIMEOUT                   true
#define CONFIG_TIMEOUT_RETRYTIMES_BEFORE_RESET    5

// Config Timeout 120s (default 60s)
#define CONFIG_TIMEOUT                            120000L

#define USE_DYNAMIC_PARAMETERS              true

#include <ESP_WiFiManager_Lite.h>

#if ESP8266 
  #define HOST_NAME   "ESP8266-Controller"
#else
  #define HOST_NAME   "ESP32-Controller"
#endif

#ifdef LED_BUILTIN
  #define LED_PIN     LED_BUILTIN
#else
  #define LED_PIN     13
#endif

#endif      //defines_h
```
---

#### 3. File [Credentials.h](examples/ESP_WiFi/Credentials.h)

```cpp
#ifndef Credentials_h
#define Credentials_h

#include "defines.h"

/// Start Default Config Data //////////////////

/*
#define SSID_MAX_LEN      32
//From v1.0.3, WPA2 passwords can be up to 63 characters long.
#define PASS_MAX_LEN      64

typedef struct
{
  char wifi_ssid[SSID_MAX_LEN];
  char wifi_pw  [PASS_MAX_LEN];
}  WiFi_Credentials;

#define NUM_WIFI_CREDENTIALS      2

// Configurable items besides fixed Header, just add board_name 
#define NUM_CONFIGURABLE_ITEMS    ( ( 2 * NUM_WIFI_CREDENTIALS ) + 1 )
////////////////

typedef struct Configuration
{
  char header         [16];
  WiFi_Credentials  WiFi_Creds  [NUM_WIFI_CREDENTIALS];
  char board_name     [24];
  int  checkSum;
} ESP_WM_LITE_Configuration;
*/

#define TO_LOAD_DEFAULT_CONFIG_DATA      false

#if TO_LOAD_DEFAULT_CONFIG_DATA

// This feature is primarily used in development to force a known set of values as Config Data
// It will NOT force the Config Portal to activate. Use DRD or erase Config Data with ESP_WiFiManager.clearConfigData()

// Used mostly for development and debugging. FORCES default values to be loaded each run.
// Config Portal data input will be ignored and overridden by DEFAULT_CONFIG_DATA
//bool LOAD_DEFAULT_CONFIG_DATA = true;

// Used mostly once debugged. Assumes good data already saved in device.
// Config Portal data input will be override DEFAULT_CONFIG_DATA
bool LOAD_DEFAULT_CONFIG_DATA = false;


ESP_WM_LITE_Configuration defaultConfig =
{
  //char header[16], dummy, not used
#if ESP8266 
  "ESP8266",
#else
  "ESP32",
#endif

  // WiFi_Credentials  WiFi_Creds  [NUM_WIFI_CREDENTIALS];
  // WiFi_Credentials.wifi_ssid and WiFi_Credentials.wifi_pw
  "SSID1",  "password1",
  "SSID2",  "password2",
  //char board_name     [24];

#if ESP8266 
  "ESP8266-Control",
#else
  "ESP32-Control",
#endif

  // terminate the list
  //int  checkSum, dummy, not used
  0
  /////////// End Default Config Data /////////////
};

#else

bool LOAD_DEFAULT_CONFIG_DATA = false;

ESP_WM_LITE_Configuration defaultConfig;

#endif    // TO_LOAD_DEFAULT_CONFIG_DATA

/////////// End Default Config Data /////////////


#endif    //Credentials_h
```
---

#### 4. File [dynamicParams.h](examples/ESP_WiFi/dynamicParams.h)

```cpp
#ifndef dynamicParams_h
#define dynamicParams_h

#include "defines.h"

// USE_DYNAMIC_PARAMETERS defined in defined.h

/////////////// Start dynamic Credentials ///////////////

//Defined in <WiFiManager_Generic_Lite.h>
/**************************************
  #define MAX_ID_LEN                5
  #define MAX_DISPLAY_NAME_LEN      16

  typedef struct
  {
  char id             [MAX_ID_LEN + 1];
  char displayName    [MAX_DISPLAY_NAME_LEN + 1];
  char *pdata;
  uint8_t maxlen;
  } MenuItem;
**************************************/

#if USE_DYNAMIC_PARAMETERS

#define MAX_BLYNK_SERVER_LEN      34
#define MAX_BLYNK_TOKEN_LEN       34

char Blynk_Server1 [MAX_BLYNK_SERVER_LEN + 1]  = "account.duckdns.org";
char Blynk_Token1  [MAX_BLYNK_TOKEN_LEN + 1]   = "token1";

char Blynk_Server2 [MAX_BLYNK_SERVER_LEN + 1]  = "account.ddns.net";
char Blynk_Token2  [MAX_BLYNK_TOKEN_LEN + 1]   = "token2";

#define MAX_BLYNK_PORT_LEN        6
char Blynk_Port   [MAX_BLYNK_PORT_LEN + 1]  = "8080";

#define MAX_MQTT_SERVER_LEN      34
char MQTT_Server  [MAX_MQTT_SERVER_LEN + 1]   = "mqtt.duckdns.org";

MenuItem myMenuItems [] =
{
  { "sv1", "Blynk Server1", Blynk_Server1,  MAX_BLYNK_SERVER_LEN },
  { "tk1", "Token1",        Blynk_Token1,   MAX_BLYNK_TOKEN_LEN },
  { "sv2", "Blynk Server2", Blynk_Server2,  MAX_BLYNK_SERVER_LEN },
  { "tk2", "Token2",        Blynk_Token2,   MAX_BLYNK_TOKEN_LEN },
  { "prt", "Port",          Blynk_Port,     MAX_BLYNK_PORT_LEN },
  { "mqt", "MQTT Server",   MQTT_Server,    MAX_MQTT_SERVER_LEN },
};

uint16_t NUM_MENU_ITEMS = sizeof(myMenuItems) / sizeof(MenuItem);  //MenuItemSize;

#else

MenuItem myMenuItems [] = {};

uint16_t NUM_MENU_ITEMS = 0;

#endif    //USE_DYNAMIC_PARAMETERS


#endif      //dynamicParams_h
```
---
---


### Debug Terminal output Samples

### 1. [ESP_WiFi](examples/ESP_WiFi) on ESP32_DEV

This is the terminal output when running [**ESP_WiFi**](examples/ESP_WiFi) example on **ESP32_DEV**:

#### 1.1. MRD/DRD => Open Config Portal

```
Starting ESP_WiFi using LittleFS on ESP32_DEV
ESP_WiFiManager_Lite v1.0.0
ESP_MultiResetDetector v1.1.1
LittleFS Flag read = 0xFFFC0003
multiResetDetectorFlag = 0xFFFC0003
lowerBytes = 0x0003, upperBytes = 0x0003
multiResetDetected, number of times = 3
Saving config file...
Saving config file OK
[WML] Multi or Double Reset Detected
[WML] Hostname=ESP32-Controller
[WML] LoadCfgFile 
[WML] OK
[WML] ======= Start Stored Config Data =======
[WML] Hdr=ESP_WM_LITE,SSID=HueNet1,PW=12345678
[WML] SSID1=HueNet2,PW1=12345678
[WML] BName=ESP32_WM_Lite
[WML] i=0,id=sv1,data=account.duckdns.org
[WML] i=1,id=tk1,data=token1
[WML] i=2,id=sv2,data=account.ddns.net
[WML] i=3,id=tk2,data=token2
[WML] i=4,id=prt,data=8080
[WML] i=5,id=mqt,data=mqtt.duckdns.org
[WML] CCSum=0x137c,RCSum=0x137c
[WML] LoadCredFile 
[WML] CrR:pdata=new.duckdns.org,len=34
[WML] CrR:pdata=token1,len=34
[WML] CrR:pdata=new.ddns.net,len=34
[WML] CrR:pdata=token2,len=34
[WML] CrR:pdata=8080,len=6
[WML] CrR:pdata=mqtt.duckdns.org,len=34
[WML] OK
[WML] CrCCsum=0x163b,CrRCsum=0x163b
[WML] Valid Stored Dynamic Data
[WML] Hdr=ESP_WM_LITE,SSID=HueNet1,PW=12345678
[WML] SSID1=HueNet2,PW1=12345678
[WML] BName=ESP32_WM_Lite
[WML] i=0,id=sv1,data=new.duckdns.org
[WML] i=1,id=tk1,data=token1
[WML] i=2,id=sv2,data=new.ddns.net
[WML] i=3,id=tk2,data=token2
[WML] i=4,id=prt,data=8080
[WML] i=5,id=mqt,data=mqtt.duckdns.org
[WML] Check if isForcedCP
[WML] LoadCPFile 
[WML] OK
[WML] bg: isForcedConfigPortal = false
[WML] bg:Stay forever in CP:DRD/MRD
[WML] clearForcedCP
[WML] SaveCPFile 
[WML] OK
[WML] SaveBkUpCPFile 
[WML] OK
[WML] 
stConf:SSID=ESP_9ABF498,PW=MyESP_9ABF498
[WML] IP=192.168.4.1,ch=10
[WML] s:millis() = 1014, configTimeout = 121014
F
Your stored Credentials :
Blynk Server1 = new.duckdns.org
Token1 = token1
Blynk Server2 = new.ddns.net
Token2 = token2
Port = 8080
MQTT Server = mqtt.duckdns.org
FFFFFFFFF
```

#### 1.2. Got valid Credentials from Config Portal then connected to WiFi

```
Starting ESP_WiFi using LittleFS on ESP32_DEV
ESP_WiFiManager_Lite v1.0.0
ESP_MultiResetDetector v1.1.1
LittleFS Flag read = 0xFFFE0001
multiResetDetectorFlag = 0xFFFE0001
lowerBytes = 0x0001, upperBytes = 0x0001
No multiResetDetected, number of times = 1
LittleFS Flag read = 0xFFFE0001
Saving config file...
Saving config file OK
[WML] LoadCfgFile 
[WML] OK
[WML] ======= Start Stored Config Data =======
[WML] Hdr=ESP_WM_LITE,SSID=HueNet1,PW=12345678
[WML] SSID1=HueNet2,PW1=12345678
[WML] BName=ESP32_WM_Lite
[WML] i=0,id=sv1,data=account.duckdns.org
[WML] i=1,id=tk1,data=token1
[WML] i=2,id=sv2,data=account.ddns.net
[WML] i=3,id=tk2,data=token2
[WML] i=4,id=prt,data=8080
[WML] i=5,id=mqt,data=mqtt.duckdns.org
[WML] LoadCredFile 
[WML] OK
[WML] Valid Stored Dynamic Data
[WML] Hdr=ESP_WM_LITE,SSID=HueNet1,PW=12345678
[WML] SSID1=HueNet2,PW1=12345678
[WML] BName=ESP32_WM_Lite
[WML] i=0,id=sv1,data=new.duckdns.org
[WML] i=1,id=tk1,data=token1
[WML] i=2,id=sv2,data=new.ddns.net
[WML] i=3,id=tk2,data=token2
[WML] i=4,id=prt,data=8080
[WML] i=5,id=mqt,data=mqtt.duckdns.org
[WML] LoadCPFile 
[WML] OK
H
Your stored Credentials :
Blynk Server1 = new.duckdns.org
Token1 = token1
Blynk Server2 = new.ddns.net
Token2 = token2
Port = 8080
MQTT Server = mqtt.duckdns.org
Stop multiResetDetecting
Saving config file...
Saving config file OK
HHHHHHHHH HHHHHHHHHH HHHHHHHHHH HHHHHHHHHH HHHHHHHHHH HHHHHHHHHH HHHHH
```

#### 1.3. Lost a WiFi and autoconnect to another WiFi AP

```
[WML] run: WiFi lost. Reconnect WiFi
[WML] Connecting MultiWifi...
[WML] WiFi connected after time: 2
[WML] SSID=HueNet2,RSSI=-54
[WML] Channel=4,IP=192.168.2.82
[WML] run: WiFi reconnected
H
HHHHHHHHHH HHHHHHHHHH
```

---

### 2. [ESP_WiFi_MQTT](examples/ESP_WiFi_MQTT) on ESP8266_NODEMCU

This is the terminal output when running [**ESP_WiFi_MQTT**](examples/ESP_WiFi_MQTT) example on **ESP8266_NODEMCU**:

#### 2.1. No Config Data => Open Config Portal

```
Starting ESP_WiFi_MQTT using LittleFS on ESP8266_NODEMCU
ESP_WiFiManager_Lite v1.0.0
ESP_MultiResetDetector v1.1.1
LittleFS Flag read = 0xFFFE0001
multiResetDetectorFlag = 0xFFFE0001
lowerBytes = 0x0001, upperBytes = 0x0001
No multiResetDetected, number of times = 1
LittleFS Flag read = 0xFFFE0001
Saving config file...
Saving config file OK
[WML] Hostname=ESP8266-Controller
[WML] LoadCfgFile 
[WML] OK
[WML] ======= Start Stored Config Data =======
[WML] Hdr=ESP_WM_LITE,SSID=HueNet1,PW=12345678
[WML] SSID1=HueNet2,PW1=12345678
[WML] BName=ESP8266_WM_Lite
[WML] i=0,id=svr,data=io.adafruit.com
[WML] i=1,id=prt,data=1883
[WML] i=2,id=usr,data=private
[WML] i=3,id=key,data=private
[WML] i=4,id=pub,data=/feeds/Temperature
[WML] i=5,id=sub,data=/feeds/LED_Control
[WML] LoadCredFile 
[WML] OK
[WML] Invalid Stored Dynamic Data. Ignored
[WML] InitCfgFile,sz=236
[WML] SaveCfgFile 
[WML] WCSum=0xda0
[WML] OK
[WML] SaveBkUpCfgFile 
[WML] OK
[WML] SaveCredFile 
[WML] OK
[WML] CrWCSum=0xc30
[WML] SaveBkUpCredFile 
[WML] OK
[WML] LoadCPFile 
[WML] OK
[WML] bg:Stay forever in CP:No ConfigDat
[WML] SaveCPFile 
[WML] OK
[WML] SaveBkUpCPFile 
[WML] OK
N
Your stored Credentials :
AIO_SERVER = blank
AIO_SERVERPORT = blank
AIO_USERNAME = blank
AIO_KEY = blank
AIO_PUB_TOPIC = blank
AIO_SUB_TOPIC = blank
NStop multiResetDetecting
Saving config file...
Saving config file OK
NNN 
```

#### 2.2. Got valid Credentials from Config Portal then connected to WiFi

```
[WML] h:UpdLittleFS
[WML] SaveCfgFile 
[WML] WCSum=0x13a5
[WML] OK
[WML] SaveBkUpCfgFile 
[WML] OK
[WML] SaveCredFile 
[WML] OK
[WML] CrWCSum=0x2236
[WML] SaveBkUpCredFile 
[WML] OK
[WML] h:Rst


Starting ESP_WiFi_MQTT using LittleFS on ESP8266_NODEMCU
ESP_WiFiManager_Lite v1.0.0
ESP_MultiResetDetector v1.1.1
LittleFS Flag read = 0xFFFE0001
multiResetDetectorFlag = 0xFFFE0001
lowerBytes = 0x0001, upperBytes = 0x0001
No multiResetDetected, number of times = 1
LittleFS Flag read = 0xFFFE0001
Saving config file...
Saving config file OK
[WML] Hostname=ESP8266-Controller
[WML] LoadCfgFile 
[WML] OK
[WML] ======= Start Stored Config Data =======
[WML] Hdr=ESP_WM_LITE,SSID=HueNet1,PW=12345678
[WML] SSID1=HueNet2,PW1=12345678
[WML] BName=ESP8266_WM_MQTT
[WML] i=0,id=svr,data=io.adafruit.com
[WML] i=1,id=prt,data=1883
[WML] i=2,id=usr,data=private
[WML] i=3,id=key,data=private
[WML] i=4,id=pub,data=/feeds/Temperature
[WML] i=5,id=sub,data=/feeds/LED_Control
[WML] LoadCredFile 
[WML] OK
[WML] Valid Stored Dynamic Data
[WML] Hdr=ESP_WM_LITE,SSID=HueNet1,PW=12345678
[WML] SSID1=HueNet2,PW1=12345678
[WML] BName=ESP8266_WM_MQTT
[WML] i=0,id=svr,data=io.adafruit.com
[WML] i=1,id=prt,data=1883
[WML] i=2,id=usr,data=user_name
[WML] i=3,id=key,data=aio_key
[WML] i=4,id=pub,data=/feeds/Temperature
[WML] i=5,id=sub,data=/feeds/LED_Control
[WML] LoadCPFile 
[WML] OK
[WML] Connecting MultiWifi...
[WML] WiFi connected after time: 1
[WML] SSID=HueNet1,RSSI=-43
[WML] Channel=2,IP=192.168.2.98
[WML] bg: WiFi OK.

Creating new WiFi client object OK
Creating new MQTT object OK
AIO_SERVER = io.adafruit.com, AIO_SERVERPORT = 1883
AIO_USERNAME = user_name, AIO_KEY = aio_key
Creating new MQTT_Pub_Topic, Temperature = user_name/feeds/Temperature
Creating new Temperature object OK
Temperature MQTT_Pub_Topic = user_name/feeds/Temperature
Creating new AIO_SUB_TOPIC, LED_Control = user_name/feeds/LED_Control
Creating new LED_Control object OK
LED_Control AIO_SUB_TOPIC = user_name/feeds/LED_Control

Connecting to WiFi MQTT (3 attempts)...
WiFi MQTT connection successful!
TW
Your stored Credentials :
AIO_SERVER = io.adafruit.com
AIO_SERVERPORT = 1883
AIO_USERNAME = user_name
AIO_KEY = aio_key
AIO_PUB_TOPIC = /feeds/Temperature
AIO_SUB_TOPIC = /feeds/LED_Control
Stop multiResetDetecting
Saving config file...
Saving config file OK
TWTWTWTW TW
```

#### 2.3. Lost a WiFi and autoconnect to another WiFi AP

```
[WML] run: WiFi lost. Reconnect WiFi
[WML] Connecting MultiWifi...
[WML] WiFi connected after time: 1
[WML] SSID=HueNet2,RSSI=-54
[WML] Channel=4,IP=192.168.2.98
[WML] run: WiFi reconnected
H
```
---
---

### Debug

Debug is enabled by default on Serial. To disable, add at the beginning of sketch

```cpp
/* Comment this out to disable prints and save space */
#define ESP_WM_LITE_DEBUG_OUTPUT      Serial

#define _ESP_WM_LITE_LOGLEVEL_        3

#define USING_MRD                     true

#if USING_MRD
  #define MULTIRESETDETECTOR_DEBUG      true
#else
  #define DOUBLERESETDETECTOR_DEBUG     true
#endif
```

---

### Troubleshooting

If you get compilation errors, more often than not, you may need to install a newer version of the board's core or this library version.

---
---

## Releases

### Release v1.0.0

1. Initial release to support ESP32 and ESP8266.

---

### Issues

Submit issues to: [ESP_WiFiManager_Lite issues](https://github.com/khoih-prog/ESP_WiFiManager_Lite/issues)

---
---

### TO DO

1. Support more boards, shields and libraries
2. Bug Searching and Killing

---

### DONE

 1. Permit EEPROM size and location configurable to avoid conflict with others.
 2. More flexible to configure reconnection timeout.
 3. For fresh config data, don't need to wait for connecting timeout before entering config portal.
 4. If the config data not entered completely (SSIDs, Passwords, etc.), entering config portal
 5. Add configurable Config Portal IP, SSID and Password
 6. Change Synch XMLHttpRequest to Async
 7. Add configurable Static IP, GW, Subnet Mask and 2 DNS Servers' IP Addresses.
 8. Add checksums
 9. Add support to **ESP32 and ESP8266**
10. Add MultiWiFi features with auto(re)connect
11. Easy-to-use **Dynamic Parameters** without the necessity to write complicated ArduinoJSon functions
12. Permit to input special chars such as **%** and **#** into data fields.
13. Default Credentials and dynamic parameters
14. **Multi/DoubleDetectDetector** to force Config Portal when multi/double reset is detected within predetermined time, default 10s.
15. Configurable Config Portal Title
16. Re-structure all examples to separate Credentials / Defines / Dynamic Params / Code so that you can change Credentials / Dynamic Params quickly for each device.
17. Add Table of Contents and Version String


---
---
 
### Contributions and Thanks

Please help contribute to this project and add your name here.


---

### Contributing

If you want to contribute to this project:

- Report bugs and errors
- Ask for enhancements
- Create issues and pull requests
- Tell other people about this library

---

### License

- The library is licensed under [MIT](https://github.com/khoih-prog/ESP_WiFiManager_Lite/blob/main/LICENSE)

---

### Copyright

Copyright 2021- Khoi Hoang

