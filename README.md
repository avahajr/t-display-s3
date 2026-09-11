# T-Display-S3
 A boilerplate project for getting started with LILYGO's [T-Display-S3](https://www.lilygo.cc/products/t-display-s3) using the [PlatformIO](https://platformio.org) IDE, [Arduino](https://www.arduino.cc/) framework, and [LVGL](https://lvgl.io/) graphics library.

**Features**:
 - Takes care of initializing the hardware.
 - Uses the local RAM for a twin framebuffer setup with DMA, and Espressif's LCD driver APIs for the highest display throughput.
 - Initializes the external PSRAM where it allocates LVGL's working memory.
 - Mounts the internal flash as a storage medium and makes it available to LVLG as the "F:" drive.
 - Maps hardware keys to an LVGL keypad input device with "Up", "Down" and "Enter" key events.
 - Provides battery voltage readings in millivolts with an API call.
 - Uses the latest version of LVGL. (v9.5.0)
 
 **Partition Table**

The 16MB flash is partitioned as follows, there are 20KBs of space allocated as `nvs` key-value storage, an `otadata` partition, and two `app` partitions defined, 2.93MBs each so that OTA updates can be supported as well. Finally, 10.92MBs of space is allocated to the `storage` partition available to the user to work with.

| Name     | Type | SubType | Offset   | Size     |
|----------|------|---------|----------|----------|
| nvs      | data | nvs     | 0x9000   | 0x5000   |
| otadata  | data | ota     | 0xe000   | 0x2000   |
| app0     | app  | ota_0   | 0x10000  | 0x2f0000 |
| app1     | app  | ota_1   | 0x300000 | 0x2f0000 |
| storage  | data | spiffs  | 0x5f0000 | 0xa10000 |

## Getting Started

Check out [example.cpp](src/example/example.cpp). It's a simple physics simulation application provided as a starting point and as an artificial load for testing.

![docs/example.gif](docs/example.gif?raw=true)

### 0. Load Images into [SPIFFS](https://randomnerdtutorials.com/esp32-vs-code-platformio-spiffs/) (first time only)
To get the example up and running, first **build and upload the filesystem image**. This will mount the [`/data`](/data/) directory to the flash memory of the board (read why we do this [here](https://randomnerdtutorials.com/esp32-vs-code-platformio-spiffs/)).

```
platformio run --target buildfs --environment T-Display-S3
platformio run --target uploadfs --environment T-Display-S3 
```

### 1. Upload a program
The following command builds and runs whatever you have locally to your board.
``` 
platformio run --target upload
```

### 2. Write your own! 
To make your own project, inherit from the `application` class and override the `on_create` and `on_update` methods. 

The easiest way to do this is to just write something over `example.cpp`.

