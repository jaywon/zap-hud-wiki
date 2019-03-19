# Creating A Tool

- Every HUD tool is imported into the service worker when the service worker is restarted.
- Each tool script runs **as** the service worker with access to the same features and held by the same restrictions.
- Every HUD tool has a similar structure.

## Module Pattern

Each HUD tool is wrapped in the [module pattern](http://www.christianheilmann.com/2007/08/22/again-with-the-module-pattern-reveal-something-to-the-world). Because each tool runs within the service worker, the module pattern allows each tool to have private variables and functions, while still allowing them to expose specified variables and functions. Additionally, the tool module will be added to the service worker's _tools_ object so that each tool can be referenced in other scripts running in the service worker.

```
var Scope = (function() {
  var privateVar = "private";
  var publicVar = "public"

  function privateFunction() {
     ...
  }

  function publicFunction() {
     ...
  }

  return {
    pubVar: publicVar,
    pubFunc: publicFunction
  };
})();

self.tools[Scope.NAME] = Scope;
```

## Tool Constants

Every tool will start with a very similar set of constants at the top of the module. These constants (to one day be added from a config file, hopefully) define some basic information that is commonly used across tools. This data is defined so as to be used throughout the source of the tool.

- **NAME**: This is the name of the tool used when identifying the tool within the source. The name is the key used to when storing the tool data in IndexedDB, as well as the key used in the service worker _tools_ object.

- **LABEL**: This is how the tool will be labeled on the HUD button. It will typically be similar to the NAME.
- **DATA**: This is what will be shown when the HUD button is hovered over and expanded. It should provide helpful information about what the tool is doing, without clicking on it.
- **ICONS**: These are the names of the files used as images on the HUD button. Sometimes there only needs to be a single image, sometimes multiple.
- **DIALOG**: Often tools will need to prompt the user about some sort of action or communicate information. Those strings should be stored here.

```
var Scope = (function() {
  var NAME = "scope";
  var LABEL = "Scope";
  var DATA = {};
    DATA.IN = "In";
    DATA.OUT = "Out";
  var ICONS = {};
    ICONS.IN = "target.png";
    ICONS.OUT = "target-grey.png";
  var DIALOG = {};
    DIALOG.IN = "Remove current domain from scope?";
    DIALOG.OUT = "Add current domain to scope?";

...
```

### initializeStorage()

After the constants are defined, every tool has an `initializeStorage` function that stores the state of the tool in indexeddb. This state can be accessed via the getTool function and saved via the `saveTool` function in utils.js. All tools have similar state variables.

- **name**: the NAME of the tool
- **label**: the LABEL of the tool
- **data**: the data to display on the button
- **icon**: which icon to display on the button
- **isSelected**: a boolean whether the tool has been selected and added to a panel
- **panel**: a string indicating which panel the tool has been selected to

In the Scope example below, there is an additional property `urls` which is specific to the Scope tool. Tools will often have state information it needs to track which would be added here.

```
...
function initializeStorage() {
  var tool = {};
  tool.name = NAME;
  tool.label = LABEL;
  tool.data = DATA.OUT;
  tool.icon = ICON.OUT;
  tool.isSelected = false;
  tool.panel = "";
  tool.urls = [];

  saveTools(tool);
}
...
```

### showDialog()

After the `initializeStorage` function typically the body of the tool's functions will follow. While these will be specific to each tool, often a tool will want to display a dialog to the user when the button is pressed. There is a template for this.

- The first part figure out what message to display, and then saves that to the `config` object which will be passed to the mainDisplay frame for displaying the dialog.
- The next part is the utility function to send a postMessage to a frame, in this case the mainDisplay frame. The postMessage sends an object. This object always starts with an `action` property to indicate what the mainDisplay should do. In the case of the Scope example we would like the mainDisplay to run the showDialog function. The showDialog function is one of several functions in mainDisplay that show different types of pop ups. They all take a single `config` object as a parameter. The config object may require different properties for the different functions, but often this will include an array of objects to describe the buttons. The button objects have two properties: `text` is the text to display on the button and `id` is the id of the button that will be returned when a button is selected.
- The final handles the response from the postMessage sent via the messageFrame function. The response will contain information returned from the display, typically this will just be the id of the button selected, which can be accessed from `response.id`.

```
function showDialog(domain) {

  checkDomainInScope(domain).then(function(isInScope) {
    var config = {};

    if(!isInScope) {
      config.text = DIALOG.OUT;
      config.buttons = [
        {text:"Add",
         id:"add"},
        {text:"Cancel",
         id:"cancel"}
      ];
    }
    else {
      config.text = DIALOG.IN;
      config.buttons = [
        {text:"Remove",
         id:"remove"},
        {text:"Cancel",
         id:"cancel"}
      ];
    }

    messageFrame("mainDisplay", {action:"showDialog", config:config}).then(function(response) {

    // Handle button choice
    if (response.id === "add") {
      addToScope(domain);
    }
    else if (response.id === "remove") {
      removeFromScope(domain);
    }

  });

  }).catch(function(error) {
    console.log(Error(error));
  });
}
```

## Message and Activate Listeners

Tools are required to have listeners for two different events: `message` and `activate`.

The activate event listener simply needs to call the `initializeStorage` function.

```
self.addEventListener("activate", function(event) {
  initializeStorage();
});
```

... to be continued ...
