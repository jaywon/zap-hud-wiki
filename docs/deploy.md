# Deploying the HUD

At the moment there are no pre-built versions of the HUD, so you need to build it yourself. This will obviously change when it is released.

To run the HUD you will need a development version of ZAP. You can either build this yourself or download the latest weekly release: https://github.com/zaproxy/zaproxy/wiki/Downloads#zap-weekly

To build and deploy the HUD you will need to run the following [Gradle](https://gradle.org) command from the directory at the top of this repo:

`gradle deploy`

When you restart ZAP the new version of the HUD should be installed.
