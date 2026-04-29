# Oxline Blood Pressure Monitor to Home Assistant

## Introduction

The Oxline Pressure X Pro Blood Pressure monitor is a good monitor, but the Bluetooth interface can be cumbersome to use in practice. You have to make sure the app is open and connected to the device. 

This device is a bridge between Bluetooth and Home Assistant. The basic function is:

* Wait for a specific Blood Pressure Monitor to become available (by MAC address)
* Connect to the device and enable notifications on a characteristic that provides intermediate pressure values and, at the end, a set of complete results.
* Process the results and return:
  * Integer sensor values for:
    * systolic pressure
    * diastolic pressure
    * pulse rate in bpm
    * count of the number of measurements
  * Text sensor with a string in the following form:
    `sss\ddd,ppp,cnt`, where `sss` is the systolic pressure, `ddd` is the diastolic pressure, `ppp` is the pulse in beats per minute, and `cnt` is the count of blood pressure measurements collected.

## Set Up and Use

The firmware is compatible with any ESP32 C3 dev kit board. These boards are low-cost and capable. The code could also be ported to the S3.

To set up the device, the MAC address *must* be known. One way to find it is with a tool like [LightBlue](https://apps.apple.com/us/app/lightblue/id557428110), which can scan for advertisements. In my case, the Blood Pressure monitor was named "JPD BPM." Once the MAC address is known, this value must be entered in the file `devices/bp_wifi_adapter.yaml` and used as the value for the substitution variable `bp_monitor_mac`. Note that the semicolon between octets is required.

If you're using a different blood pressure monitor, you may also need other values for `bp_monitor_service_uuid` and `bp_monitor_characteristic_uuid`.

## Limitations

* The "User selector" on the blood pressure monitor is ignored. If using this for multiple users, another solution will be required. The hardware could be augmented to include some user-settable switches, which could be reported with the rest of the data.