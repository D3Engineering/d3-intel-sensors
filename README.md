## Patches

linux-kernel-lts: https://github.com/intel/linux-intel-lts.git

Patches for linux-kernel-lts should be applied to:
```
commit dfff915012ab62c58f33603bb3eeb3e21027b332 (tag: lts-v5.15.96-linux-230414T064922Z)
Author: Aravindhan Gunasekaran <aravindhan.gunasekaran@intel.com>
Date:   Fri Apr 14 10:38:26 2023 +0530

    Revert "igc: Fix PPS delta between two synchronized end-points"
    
    This reverts commit cfd5978411edae852636f8a6cd53ea093f292e79.
```

ipu6-camera-hal: https://github.com/intel/ipu6-camera-hal.git

Patches for ipu6-camera-hal should be applied to:
```
commit cf18f7de1ec9742aff08005e309edcebf882597a (tag: v1.0.0-adln-mr1)
Merge: e6925d6 520f484
Author: zouxiaoh <xiaohong.zou@intel.com>
Date:   Thu Mar 23 18:25:09 2023 +0800

    Merge pull request #48 from zouxiaoh/iotg_ipu6
    
    iotg adl-n mr1 release
```


## BIOS setup:

* Intel Advanced Menu ->
* System Agent (SA) Configuration ->
* MIPI Camera Configuration ->

### CAM1 control log:

* Control Logic 1: enabled
* Control Logic options ->
	* Control Logic Type: Discrete
	* CRD Version: CRD-D
	* Input Clock: 19.2 MHz
	* PCH Clock Source: IMGCLKOUT_0
	* Number of GPIO pins: 1
	* GPIO 0
		* Group Pad Number: 5
		* Group Number: R
		* Functino: Reset
		* Active Value: 1
		* Initial Value: 1

### CAM2 control log:

* Control Logic 1: enabled
* Control Logic options ->
	* Control Logic Type: Discrete
	* CRD Version: CRD-D
	* Input Clock: 19.2 MHz
	* PCH Clock Source: IMGCLKOUT_1
	* Number of GPIO pins: 1
	* GPIO 0
		* Group Pad Number: 11
		* Group Number: A
		* Functino: Reset
		* Active Value: 1
		* Initial Value: 1

### ISX031 CAM1

* Camera1: Enabled
* Link options ->
    * Sensor Model: User Custom
    * Custom HID: D3000001
    * Lanes Clock division: 4 4 2 2
    * CRD Version: CRD-D
    * GPIO control: Control Logic 1
    * Camera Position: Front
    * Flash Support: Disabled
    * Privacy LED: Disabled
    * Rotation: 0
    * Camera module name: YHCE
    * MIPI port: 2
    * LaneUsed: x4
    * PortSpeed: 4
    * MCLK: 19200000
    * EEPROM Type: ROM_NON
    * VCM Type: VCM_NONE
    * Number of I2C Components: 1
    * I2C Channel: I2C1
    * Device 0
        * I2C Address: 1A
        * Device Type: Sensor
    * Flash Driver Selection: Disabled

### ISX031 CAM2

* Camera2: Enabled
* Link options ->
    * Sensor Model: User Custom
    * Custom HID: D3000001
    * Lanes Clock division: 4 4 2 2
    * CRD Version: CRD-D
    * GPIO control: Control Logic 2
    * Camera Position: Front
    * Flash Support: Disabled
    * Privacy LED: Disabled
    * Rotation: 0
    * Camera module name: YHRN
    * MIPI port: 2
    * LaneUsed: x4
    * PortSpeed: 4
    * MCLK: 19200000
    * EEPROM Type: ROM_NON
    * VCM Type: VCM_NONE
    * Number of I2C Components: 1
    * I2C Channel: I2C5
    * Device 0
        * I2C Address: 1A
        * Device Type: Sensor
    * Flash Driver Selection: Disabled

### CIC + IMX390 Discovery

* Camera1: Enabled
* Link options ->
    * Sensor Model: User Custom
    * Custom HID: INTC10C1
    * Lanes Clock division: 4 4 2 2
    * CRD Version: CRD-D
    * GPIO control: Control Logic 1
    * Camera Position: Front
    * Flash Support: Disabled
    * Privacy LED: Disabled
    * Rotation: 0
    * Camera module name: YHCE
    * MIPI port: 2
    * LaneUsed: x2
    * PortSpeed: 2
    * MCLK: 19200000
    * EEPROM Type: ROM_NON
    * VCM Type: VCM_NONE
    * Number of I2C Components: 3
    * I2C Channel: I2C1
    * Device 0
        * I2C Address: 32
        * Device Type: Sensor
    * Device 1
        * I2C Address: 40
        * Device Type: IO Expander
    * Device 2
        * I2C Address: 20
        * Device Type: IO Expander
    * Flash Driver Selection: Disabled

### CIC + ISX031

* Camera1: Enabled
* Link options ->
    * Sensor Model: User Custom
    * Custom HID: D3000002
    * Lanes Clock division: 4 4 2 2
    * CRD Version: CRD-D
    * GPIO control: Control Logic 1
    * Camera Position: Front
    * Flash Support: Disabled
    * Privacy LED: Disabled
    * Rotation: 0
    * Camera module name: YHCE
    * MIPI port: 2
    * LaneUsed: x2
    * PortSpeed: 2
    * MCLK: 19200000
    * EEPROM Type: ROM_NON
    * VCM Type: VCM_NONE
    * Number of I2C Components: 3
    * I2C Channel: I2C1
    * Device 0
        * I2C Address: 32
        * Device Type: Sensor
    * Device 1
        * I2C Address: 40
        * Device Type: IO Expander
    * Device 2
        * I2C Address: 20
        * Device Type: IO Expander
    * Flash Driver Selection: Disabled

## Streaming cameras

```
export DISPLAY=:0
export LIBVA_DRIVERS_PATH=/usr/lib64/dri
export LIBVA_DRIVER_NAME=iHD
export PKG_CONFIG_PATH=/usr/local/lib/pkgconfig:/usr/lib64/pkgconfig:/usr/lib/pkgconfig
export LD_LIBRARY_PATH=/usr/local/lib:/use/lib64:/usr/lib
export GST_PLUGIN_PATH=/usr/lib/gstreamer-1.0/
```

### ISX031

* `gst-launch-1.0 icamerasrc device-name=isx031 ! 'video/x-raw,format=UYVY,width=1920,height=1536' ! videoconvert ! xvimagesink`
* `gst-launch-1.0 icamerasrc device-name=isx031-2 ! 'video/x-raw,format=UYVY,width=1920,height=1536' ! videoconvert ! xvimagesink`

### CIC + IMX390 Discovery

Before streaming, set TI960 link_freq to 600MHz: `v4l2-ctl -d  /dev/v4l-subdev11 -c v4l2_cid_link_freq=2`

* `gst-launch-1.0 icamerasrc device-name=isx031-fpd-0 ! 'video/x-raw,format=UYVY,width=1920,height=1536' ! videoconvert ! xvimagesink`
* `gst-launch-1.0 icamerasrc device-name=isx031-fpd-1 ! 'video/x-raw,format=UYVY,width=1920,height=1536' ! videoconvert ! xvimagesink`
* `gst-launch-1.0 icamerasrc device-name=isx031-fpd-2 ! 'video/x-raw,format=UYVY,width=1920,height=1536' ! videoconvert ! xvimagesink`
* `gst-launch-1.0 icamerasrc device-name=isx031-fpd-3 ! 'video/x-raw,format=UYVY,width=1920,height=1536' ! videoconvert ! xvimagesink`

### CIC + IMX390 Discovery

Before streaming, set TI960 link_freq to 600MHz: `v4l2-ctl -d  /dev/v4l-subdev11 -c v4l2_cid_link_freq=2`

* `gst-launch-1.0 icamerasrc device-name=imx390 ! 'video/x-raw,format=NV12,width=1920,height=1200' ! videoconvert ! xvimagesink`
* `gst-launch-1.0 icamerasrc device-name=imx390-2 ! 'video/x-raw,format=NV12,width=1920,height=1200' ! videoconvert ! xvimagesink`
* `gst-launch-1.0 icamerasrc device-name=imx390-3 ! 'video/x-raw,format=NV12,width=1920,height=1200' ! videoconvert ! xvimagesink`
* `gst-launch-1.0 icamerasrc device-name=imx390-4 ! 'video/x-raw,format=NV12,width=1920,height=1200' ! videoconvert ! xvimagesink`

Once streaming, enable sub-pixel lens shading:

* Pro-medium, 52N or 74N: `v4l2-ctl -d  /dev/v4l-subdev10 -c test_pattern=3`