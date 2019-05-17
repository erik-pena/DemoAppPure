# Repro

## Preliminary Details

Ensure that you have You.i Engine 5.2.0 installed and available on your machine.

This repro was generated on a [Roku Ultra](https://www.roku.com/products/roku-ultra), model: 4660X2

## Create a build of You.i Engine (Cloud)

Build the You.i Engine by running the following commands--

```bash
$ cd ./youi/
$ ./generate.rb -p osx -b ./build/roku -d YI_BUILD_CLOUD_SERVER=ON
$ ./build.rb -b ./build/roku/
```

## Run You.i Engine (Cloud)

Next, open the Xcode project for the You.i Engine--

```bash
$ cd ./build/roku/
$ open DemoAppPure.xcodeproj
```

Select the appropriate "DemoAppPure > My Mac" scheme and click the run button.  You.i Engine is ready when you see the following in the Xcode console--

```text
[2019-05-17 01:44:04:033]  I/CYICloudServer: Start Listening on port TCP(54323) and WebSocket(54323). Now WAITING_FOR_CLIENT.
```

## Deploy the Roku App

With the You.i Engine running from the last step, run the following commands to deploy the Roku client to the Roku device (be sure to replace `<ROKU_IP_ADDRESS>`, `<PASSWORD>` and `<YOUI_ENGINE_IP>` with appropriate values)--

```bash
$ cd ./../../../rokuclient/
$ ./build.rb -r <ROKU_IP_ADDRESS> -e ~/youiengine/5.2.0 -u rokudev:<PASSWORD> -s <YOUI_ENGINE_IP>:54323
```

## Observations

The television shows the default spinner while loading and eventually disappears with a stock Roku message reading--

```text
Oops...  there was a problem.  Sorry for the
inconvenience!
Here's what happened:

- We've lost the connection to the remote
  server... which is bad
- The error code was 32
```

Included are also the logs that were spat out by using `telnet` against the Roku device--

[roku-telnet.log](./roku-telnet.log)

Additionally the logs that were output in the Xcode Console for the You.i Engine--

[xcode-engine.log](./xcode-engine.log)

-------

Lastly, if I omit the port number off of the `-s` argument when attempting the deploy the app--

```bash
./build.rb -r <ROKU_IP_ADDRESS> -e ~/youiengine/5.2.0 -u rokudev:<PASSWORD> -s <YOUI_ENGINE_IP>
```

I get an RSOD with a message that reads--

```text
Did not receive an http response from the server.
```
