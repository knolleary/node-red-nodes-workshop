*Quick links :*
[Home](/README.md) - [**Part 1**](../part1/README.md) - [Part 2](../part2/README.md) - [Part 3](../part3/README.md) - [Part 4](../part4/README.md) - [Part 5](../part5/README.md) - [Part 6](../part6/README.md)
***

# Part 2: Creating your first node

**Goal:** *Create a very simple node and get it running inside Node-RED*

## Introduction

This section of the workshop gets the basic structure of your node in place.

Nodes require two files:
 - a JavaScript file that is loaded into the Node-RED runtime to provide the runtime behaviour
 - an HTML file that is loaded into the editor to provide the edit-time behaviour

## Steps

 - [1 - Create the JavaScript file](#21---create-the-javascript-file)
 - [2 - Create the HTML file](#22---create-the-html-file)
 - [3 - Update package.json](#23---update-packagejson)
 - [4 - Check your node is loading](#24---check-your-node-is-loading)

## 2.1 - Create the JavaScript file

Nodes are wrapped as a Node.js module. The module exports a function that gets called
when the runtime loads the node on start-up. The function is called with a
single argument, `RED`, that provides the module access to the Node-RED runtime api.

1. Create a file called `node.js` in the `workshop-node` directory, with the following contents:

```javascript
module.exports = function(RED) {

    // Define a constructor function for the node. This is called
    // when the node is created in the runtime.
    function WorkshopNode(config) {
        RED.nodes.createNode(this,config);
        const node = this;

        // Add a handler for the 'input' event - called whenever this node
        // is sent a message
        node.on('input', function(msg) {

            // By convention, messages have a `payload` property.
            // This example coerces it to a string and converts to lower case
            msg.payload = ('' + msg.payload).toLowerCase();

            // Send the message on
            node.send(msg);
        });
    }

    // Register this node function with the runtime - along with
    // the node type name.
    RED.nodes.registerType("workshop-node",WorkshopNode);
}
```

Key points to note from this code:

 - It exports a single function that gets called when the module is loaded by
   the runtime.
 - A node is defined by a function that will get called when the runtime needs
   to create an instance of it.
 - There's some required boilerplate - `RED.nodes.createNode(this, config)`. Don't
   leave home without it.
 - The `input` event is called whenever a message is sent to the node. That's
   where the magic happens.
 - The node function is registerd with the runtime with the node type name - `workshop-node`
   in this example. That is important as it is how the runtime identifies the node
   type.

What may be less obvious is that a single JavaScript file can contain multiple 
types of node - by making multiple calls to `RED.nodes.registerType`.


## 2.2 - Create the HTML file

Before we can test this code, we need the node's HTML file as well.

1. Create a file called `node.html` in the `workshop-node` directory, with the following contents:

```html
<script type="text/javascript">
    RED.nodes.registerType('workshop-node',{
        category: 'function',
        color: '#a6bbcf',
        defaults: {
            name: {value:""}
        },
        inputs:1,
        outputs:1,
        icon: "file.png",
        label: function() {
            return this.name||"workshop-node";
        }
    });
</script>

<script type="text/html" data-template-name="workshop-node">
    <div class="form-row">
        <label for="node-input-name"><i class="fa fa-tag"></i> Name</label>
        <input type="text" id="node-input-name" placeholder="Name">
    </div>
</script>

<script type="text/html" data-help-name="workshop-node">
    <p>A simple node that converts message payloads into all lower-case characters</p>
</script>
```

A node’s HTML file provides the following things:
 - the main node definition that is registered with the editor
 - the edit template
 - the help text

In this example, the node has a single editable property, name. Whilst not required,
there is a widely used convention to this property to help distinguish between
multiple instances of a node in a single flow. We'll be revisiting node properties
in the next step of the workshop.

Key points to note from this code:

 - The `RED.nodes.registerType` call includes the node type name (`workshop-node`)
   as the first argument.
 - The edit template `<script>` has a `data-template-name` attribute set to `workshop-node`
 - The help text `<script>` has a `data-help-name` attribute also set to the node type

## 2.3 - Update package.json

The final piece needed to enable Node-RED to load your node is to add some meta-data
to the `package.json`. This is was Node-RED is looking for when it scans for nodes
to load.

1. In your existing `package.json` file, add a `node-red` section as follows:

    ```json
    {
        "name" : "...",
        "version": "...",
        ...
        "node-red" : {
            "nodes": {
                "workshop-node": "node.js"
            }
        },
        "keywords": ["node-red"]
    }
    ```

    Note the `keywords` section has `"node-red"` listed. This is used by the
    [Node-RED Flow Library](https://flows.nodered.org) to identify this as a module
    that provides Node-RED nodes.

## 2.4 - Check your node is loading

Once you get this far you should have everything in place needed for Node-RED to
load the node.

Restart Node-RED, load the editor and find your node in the palette.

If your node doesn't appear, check the Node-RED log for any errors reported for your node.
Otherwise, step back through the previous steps to ensure you've got everything in place.

Create a flow in the editor using an Inject node, your new node and a Debug node:

```
[Inject] --> [Workshop Node] --> [Debug]
```

Configure the Inject node to send a string of "HELLO" - or anything you want. Just
make sure it isn't all lower-case already as that's what your new node will be doing.

Deploy the flow, click the Inject node and check the Debug side to see the result - you
should see the payload has been converted to lower case letters.


## Summary

In this section of the workshop you have:

 - created the core files required to create a Node-RED node
 - verified the node is loading in the editor and working


## Next Steps

With the core structure of the node in place, the next task is to add some configurable
options to the node. This is what we'll do in [Part 3](../part2/README.md).


***
*Quick links :*
[Home](/README.md) - [**Part 1**](../part1/README.md) - [Part 2](../part2/README.md) - [Part 3](../part3/README.md) - [Part 4](../part4/README.md) - [Part 5](../part5/README.md) - [Part 6](../part6/README.md)