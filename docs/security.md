# Security

The HUD is still in alpha, and so you are recommended not to use it on sites you do not trust at this stage.

However we do aim to make the HUD suitably secure and it therefore is in scope for the ZAP bug bounty on [BugCrowd](https://bugcrowd.com/owaspzap)

The HUD has the following security features to ensure that malicious sites cannot attack the user via the HUD:

- The ZAP API is only accessed via WebSockets
- The WebSockets endpoint only accepts connections from the https://zap domain
- All postMessage listeners check the message origin
- All displayed data is suitably escaped

Messages sent from the target domain to the ZAP domain:

- Require shared secrets that are protected by closures
- Are not trusted and strictly parsed
- Can be completely disabled if the HUD is to be used with untrusted sites

Note that when using the HUD you should not allow ZAP to be accessed by arbitrary IP addresses or disable any of the ZAP API security features. The default settings are safe but can be changed via the [API Options screen](https://github.com/zaproxy/zap-core-help/wiki/HelpUiDialogsOptionsApi).

For even more security you might want to run ZAP in a container like [Docker](/userguide/using-the-hud/#launch-with-docker).
