*Quick links :*
[Home](/README.md) - [Part 1](../part1/README.md) - [Part 2](../part2/README.md) - [**Part 3**](../part3/README.md) - [Part 4](../part4/README.md) - [Part 5](../part5/README.md) - [Part 6](../part6/README.md)
***

# Part 3: Adding properties to the node

**Goal:** *Make your node customisable to change its behaviour*

## Introduction

This section of the workshop makes you node customisable by adding properties
that can be set within the editor.

Node properties are defined in a object called `defaults` that is included in
the node definition passed in to the call to `RED.nodes.registerType` in the node's
HTML file.

In the code you added in the previous part of this workshop, the node had one property
defined - `name`. This is a standard convention for all nodes to have a `name` property.

The `defaults` object lists the node properties with additional meta-data - such
as the default value the property should be given when a new instance of the node
is added in the editor.

```javascript
    defaults: {
        name: { value: "" }
    },
```

To enable a user to edit the value of the property, it needs to be added to 
the node's edit dialog.

This is also done in the html file - within the `<script>` block that has the
`data-template-name` attribute.

Node-RED provides some standard CSS styles to use when creating the node edit
dialog.

In the code you've got, you'll see the edit template looks like this:

```html
<div class="form-row">
    <label for="node-input-name"><i class="fa fa-tag"></i> Name</label>
    <input type="text" id="node-input-name" placeholder="Name">
</div>
```

The key point here is the `<input>` for the name property has the id `node-input-name`.

For convenience, the editor will automatically populate any `<input>` (`text`, `password`, `checkbox` etc)
or `<select>` based on its `id` attribute. For each property defined in the `defaults`
object, the editor will look for a corresponding form element with an id of `node-input-[property-name]`.


## Steps

 - [1 - Add a new property to the node](#31---add-a-new-property-to-the-node)
 - [2 - Updating the runtime behaviour](#32---updating-the-runtime-behaviour)

## 3.1 - Add a new property to the node

For this part of the workshop, we're going to allow the user to pick if the node
should convert payloads to lower case (the current behaviour), or to convert to
upper case.

1. Add a new property called `mode` to the `defaults` object.

    ```javascript
    defaults: {
        name: { value: "" },
        mode: { value: "lc" }
    },
    ```

2. Add a select box to the node's edit dialog for this property:

    ```html
    <div class="form-row">
        <label for="node-input-mode"><i class="fa fa-text-o"></i> Action</label>
        <select id="node-input-mode">
            <option value="lc">Lower-Case</option>
            <option value="uc">Upper-Case</option>
        </select>
    </div>
    ```

Note the icons available to use can be selected from [FontAwesome 4.7](https://fontawesome.com/v4/icons/).

3. Restart Node-RED and reload the editor. Add a new instance of the node to
   you workspace and double click on it to open the editor. Check the edit dialog
   looks correct and that you can set values, deploy changes and see those
   changes get saved

## 3.2 - Updating the runtime behaviour

Being able to set the property in the editor is only useful once the runtime
knows what to do with it in the JavaScript file.

The current code defines the `WorkshopNode` constructor function. This function
takes a single argument - the node's configuration.

1. Update the constructor function to keep a local copy of the configuration properties:

    ```javascript
    function WorkshopNode(config) {
        RED.nodes.createNode(this,config);
        // Store the mode property locally. In case the value isn't set, default to
        // lower-case mode (lc).
        this.mode = config.mode || 'lc'
    ```

2. Update the `input` handler function to vary its behaviour passed on the property value:

    ```javascript
    node.on('input', function(msg) {
        const strPayload = ('' + msg.payload)
        if (node.mode === 'lc') {
            msg.payload = strPayload.toLowerCase()
        } else if (node.mode === 'uc') {
            msg.payload = strPayload.toUpperCase()
        }
        node.send(msg);
    })
    ```

3. Restart Node-RED, reload the editor and verify you can change the mode property
in the node, deploy the changes and see them in action.


## Summary

In this section of the workshop you have:

 - added a new property to the node by updating the edit template and node definition
 - updated the node to change its behaviour based on that property


## Next Steps

Before adding too many features we should create some tests for the node. In [Part 4](../part4/README.md)
we'll look at how to do that.

***
*Quick links :*
[Home](/README.md) - [Part 1](../part1/README.md) - [Part 2](../part2/README.md) - [**Part 3**](../part3/README.md) - [Part 4](../part4/README.md) - [Part 5](../part5/README.md) - [Part 6](../part6/README.md)