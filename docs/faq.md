# Frequently Asked Questions

## Which browsers are supported?

The ZAP HUD should work with any modern web browsers. To start with we are targetting the latest versions of Firefox and Chrome.

## Will sites be able to detect if the HUD is being used?

Yes, the HUD does make some small changes to target sites, so it will be possible for sites to detect that the HUD is being used.

## Will sites be able to break the HUD?

Yes, it will be possible for sites to deliberately break the HUD. However as ZAP is in front of the site then it will be possible to undo anything that the site does to break the HUD. In the end its an arms race, but the HUD user will always be at an advantage. We also hope that developers will choose to spend time making their sites more secure than trying to break security tools that are designed to help them.

## Will sites be able to attack HUD users?

The HUD is designed to be as secure as possible. Once released the HUD will be part of the ZAP bounty program so Remote Code Execution vulnerabilities will be eligible for a bounty.

## What if the target application has strong CSP rules preventing 3rd party javascript?

The injected javascript file’s domain is the same as the target application . To the browser it looks like it is served from the same domain as the rest of the application. When the browser sends the request, ZAP intercepts it, recognizes the unique key appended to the filename that was added when ZAP originally injected it, and serves the response itself. Both the browser and application server are none the wiser.

## What if there are CSP rules against running javascript at all?

ZAP uses its position as a proxy to strip out or modify any headers that would prevent the HUD from running. ZAP also strips out headers such as the referrer-policy and cache-control headers as well as several CSP directives.

## What if the injected HUD javascript or iframes interfere with target application?

The injected javascript is surrounded by a javascript closure that encapsulates it in its own scope to prevent naming conflicts and the injected contents from being accessed by any other script. The only change the target application will notice is that there is an extra javascript variable and a few iframes in the DOM. Presenting the UI via overlaid iframes also helps to not skew the target application. The HUD even serves the iframe contents on a separate domain so that single origin policy can further isolate the code. Keeping a low application footprint is a key design aspect of the HUD, so that we avoid breaking the target application and skewing our own scan results.

## If you’re injecting the Heads Up Display into every HTML file returned by the server, won’t you have trouble with applications that use iframes?

To prevent having HUD’s running in iframes the first thing the inject script does is check a property of the DOM window object to see if the injected window is the “top” window. There is only ever a single “top” window in a tab and that window is the highest in the window hierarchy.
