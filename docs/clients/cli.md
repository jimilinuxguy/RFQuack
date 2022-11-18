!!! warning
    This section is fairly incomplete.

To talk to your RFQuack dongle, you have two options:

- MQTT
- Serial

Let's see the pros and cons of each.

## MQTT Transport

(and hardware serial console)

Install or have access to an MQTT broker (Mosquitto is just perfect for this):

* PROs
    * you don't need cables (hint: your RFQuack hardware can be battery powered)
    * if you want to connect the RFQuack hardware to your computer, you get a free (hardware) serial console for monitoring on the USB port
* CONs
    * you need network connectivity (WiFi or cellular)
    * there's latency

## Hardware Serial Transport

(and software serial console)

Connect the dongle via USB

* PROs
    * there's little latency
    * you don't need to rely on network stability
* CONs
    * if you want full monitoring and debugging capabilities, you'll need to hookup a UART cable to the RFQuack hardware
        * by default, a software serial device is used, and will write on pins 16, 12 (RX, TX)
        * this can be changed by defining `RFQUACK_LOG_SS_RX_PIN` and `RFQUACK_LOG_SS_TX_PIN` before `#include <rfquack.h>`
    * your range is limited by the length of your USB cable (you don't say! 😮)

## Command Line Interface

Now you can use RFQuack via the IPython-based shell. We highly recommend using a second console open to keep an eye on the output log.

### Test the Shell

```shell
$ rfq --help
Usage: rfquack [OPTIONS] COMMAND [ARGS]...

Options:
  -l, --loglevel [CRITICAL|ERROR|WARNING|INFO|DEBUG|NOTSET]
  -h, --help                      Show this message and exit.

Commands:
  mqtt  RFQuack client with MQTT transport.
  tty   RFQuack client with serial transport.
```


### MQTT Transport

To use this, you'll have to build a firmware configured to use the MQTT transport. Please check the `USE_MQTT` configuration variable in this manual.
```rfq
$ rfq mqtt --help
Usage: rfquack mqtt [OPTIONS]

  RFQuack client with MQTT transport. Assumes one dongle per MQTT broker.

Options:
  -i, --client_id TEXT
  -H, --host TEXT
  -P, --port INTEGER
  -u, --username TEXT
  -p, --password TEXT
  -h, --help            Show this message and exit.
```

### Serial Transport

This is the default transport, unless `USE_MQTT` is set in the `build.env` file.

```shell
$ rfq tty --help
Usage: rfquack tty [OPTIONS]

  RFQuack client with serial transport.

Options:
  -b, --baudrate INTEGER
  -s, --bytesize INTEGER
  -p, --parity [M|S|E|O|N]
  -S, --stopbits [1|1.5|2]
  -t, --timeout INTEGER
  -P, --port TEXT           [required]
  --help                    Show this message and exit.
```

### Examples

More concretely:

```shell
$ rfq tty -P /dev/ttyUSB0
2019-04-10 18:04:31 local RFQuack[20877] INFO Transport initialized
2019-04-10 18:04:31 local RFQuack[20877] INFO Transport initialized (QoS = 2): mid = 2

...

RFQuack(/dev/ttyUSB0, 115200,8,N,1)> q.radioA.set_modem_config(modulation="OOK", carrierFreq=434.437)

result = 0
message = 2 changes applied and 0 failed.

RFQuack(/dev/ttyUSB0, 115200,8,N,1)> q.radioA.rx()

result = 0
message =
...
```

At this point you're good to go from here!
