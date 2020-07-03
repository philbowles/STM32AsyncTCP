# STM32AsyncTCP 

This is as "close as possible" port of the well-known [ArduinoIDE library ESPAsyncTCP](https://github.com/me-no-dev/ESPAsyncTCP) and I am of course immensely grateful to its author "me-no-dev" for his huge contribution to the community.

I have tried to make as few changes as possible, so that other Arduino libs that rely on ESPAsyncTCP can be ported with zero(ish) effort. It is testament to the quality and solidity of ESPAsyncTCP, the genius of the ArduinoUDE team and the STM32Duino library authors that this port didn't take long at all, and went *far* more smoothly and easily than I had expected.

The intended target is STM32-NUCLEO boards under ArduinoIDE using the ST-approved boards manager list, [version 1.9.0](https://github.com/stm32duino)

It has been tested *solely* on STM32-NUCLEOF429ZI

and requires these libraries: (follow links)

* [LwIP](https://github.com/stm32duino/LwIP)
* [STM32Ethernet](https://github.com/stm32duino/STM32Ethernet)

So if it runs on any other board, or under any other development scenarios, then it is more by luck than judgment and I am unable to comment or assist, so....

## Important point

The library is intimately dependent upon the target platform's LwIP installation. The STM32 implementation of LwIP is *very* different from both the ESP8266 and ESP32 version in terms of number of buffers, MTU size TCP_MSS size etc...so if you are going to try to post anything, *make sure* it does not assume anything re the installation.

The first lib I tried to port "on top" of ESPAsyncTCP sent six short messages, EITHER relying on the fact that the send queue on ESP/LwIP has 8 "slots" configured and knowing that the lib would amalgamate them into one packet... OR (*far* more likely) not having a clue about why or how any of it actually worked.

The STM32 installation has only 2x slots. Hence the above code just failed with "out of memory(!)" and refused to even connect, because there was no error-checking. Of course on HIS setup, if it ever fails, your net is down so it can't connect anyway, so why check?

Also that *type* of problem does not always automatically start working once platform differences have been  resolved, sometimes code will need to be rewritten if it "accidentally" or carlesssly renders itself implementation-dependent. The above example - if well/properly written - wouldn't have failed in the first place...let alone then alerted me to the several dozen other things wrong with it.. :)

Anyway: You have been warned :) Assume nothing! 

To use, change any occurence of `ESPAsyncTCP.h` to `STM32AsyncTCP.h`

## This is almost* a "straight lift" of the orignal documentation

For ESP32 look [HERE](https://github.com/me-no-dev/AsyncTCP)

[![Join the chat at https://gitter.im/me-no-dev/ESPAsyncWebServer](https://badges.gitter.im/me-no-dev/ESPAsyncWebServer.svg)](https://gitter.im/me-no-dev/ESPAsyncWebServer?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

This is a fully asynchronous TCP library, aimed at enabling trouble-free, multi-connection network environment for Espressif's ESP8266 MCUs.

This library is the base for [ESPAsyncWebServer](https://github.com/me-no-dev/ESPAsyncWebServer)

## AsyncClient and AsyncServer
The base classes on which everything else is built. They expose all possible scenarios, but are really raw and require more skills to use.

## AsyncPrinter
This class can be used to send data like any other ```Print``` interface (```Serial``` for example).
The object then can be used outside of the Async callbacks (the loop) and receive asynchronously data using ```onData```. The object can be checked if the underlying ```AsyncClient```is connected, or hook to the ```onDisconnect``` callback.

## AsyncTCPbuffer
This class is really similar to the ```AsyncPrinter```, but it differs in the fact that it can buffer some of the incoming data.

## SyncClient
It is exactly what it sounds like. This is a standard, blocking TCP Client, similar to the one included in ```ESP8266WiFi```

## Libraries and projects that use AsyncTCP
- [ESP Async Web Server](https://github.com/me-no-dev/ESPAsyncWebServer)
  
- [Async MQTT client](https://github.com/marvinroger/async-mqtt-client)
  
  * *Personally I would avoid AsynMqttClient like the plague - there are so many fatal bugs in it, that **it is not fit for purpose**. For a list of 16 of those bugs, [read this](https://github.com/philbowles/PangolinMQTT/blob/master/docs/bugs.md)*

- [arduinoWebSockets](https://github.com/Links2004/arduinoWebSockets)
- [ESP8266 Smart Home](https://github.com/baruch/esp8266_smart_home)
- [KBox Firmware](https://github.com/sarfata/kbox-firmware)
