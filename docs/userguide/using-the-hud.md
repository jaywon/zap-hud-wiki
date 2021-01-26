## Launch the HUD

1. Start by [running and configuring ZAP](https://github.com/zaproxy/zaproxy).
2. Press the ![HUD Radar](https://raw.githubusercontent.com/zaproxy/zap-hud/master/src/main/resources/org/zaproxy/zap/extension/hud/resources/radar.png)HUD add-on button to turn the HUD on.
3. Start browsing!

After the HUD is turned on, the HUD will appear as an overlay in the browser on the next page load.

## Launch with Docker

You can use the HUD with ZAP running in a [Docker](https://github.com/zaproxy/zaproxy/wiki/Docker) container for extra security or if you prefer not to install Java locally.

To do this you need to run ZAP with the following options:

```
docker run -u zap -p 9090:9090 -i owasp/zap2docker-weekly zap.sh -daemon -host 0.0.0.0 -port 9090 \
-config api.addrs.addr.name=.* -config api.addrs.addr.regex=true -config -config api.key=CHANGE_ME \
-config hud.enabledForDaemon=true
```

ZAP will run in daemon mode in Docker.

- You can change the port 9090 to a different one if you like but you must change it in all 3 places as well as in the instructions below.
- Replace the `api.key` value from `CHANGE_ME` to your own preference.

### Proxy Configuration

**Firefox or Chrome:**

- Open http://localhost:9090/UI/core/other/rootcert/ in your browser
- Enter the API key you set when starting ZAP and click on the `rootcert` button
- Save the certificate file locally
- [Import that certificate](https://github.com/zaproxy/zap-core-help/wiki/HelpUiDialogsOptionsDynsslcert#install-zap-root-ca-certificate) file into your browser as a trusted root cert
- [Configure your browser](https://github.com/zaproxy/zap-core-help/wiki/HelpStartProxies) to proxy via localhost:9090

**_Things To Note:_**

- ZAP will create a new root CA cert **every time** you create a new Docker image, so you will need to import the cert into your browser again.

- When you open new URLs in this browser you should see the HUD enabled.

- You will need to be able to access the URLs that you want to test from the Docker image. Localhost in your browser is not the same as localhost in the Docker image.

**Links for further information:**

- [How can I connect to ZAP remotely?](https://github.com/zaproxy/zaproxy/wiki/FAQremote)
- [Why is an API key required by default?](https://github.com/zaproxy/zaproxy/wiki/FAQapikey)

## Parts of the HUD

[ insert image of HUD with call outs ]

**Tools** - Many features of ZAP have been transformed into HUD tools. These tools can be used by simply clicking on them. To change options or settings for a tool simply right click on them.  
**Panels** - The HUD tools are organized into panels. Tools can be moved, arranged, added, and deleted to fit a users preference.  
**Settings** - HUD settings and options can be viewed by clicking on the gear in the upper left hand corner.  
**Alerts** - New alerts found by the passive scanner will pop up in the bottom right hand corner.
