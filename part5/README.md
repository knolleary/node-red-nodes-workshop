*Quick links :*
[Home](/README.md) - [**Part 1**](../part1/README.md) - [Part 2](../part2/README.md) - [Part 3](../part3/README.md) - [Part 4](../part4/README.md) - [Part 5](../part5/README.md) - [Part 6](../part6/README.md)
***

# Part 5: Wrapping an existing npm module

**Goal:** *Add more functionality to the node using an existing npm module*

## Introduction

You hopefully don't need me to explain how much functionality is available within
the npm ecosystem. There's a module for everything.

As Node-RED nodes are just modules themselves, they are able to make use of the
full range of what's available in npm.

For a low-code tool like Node-RED, having all that functionality available is very
valuable - as long as it is exposed properly to end users.

Nodes get to be opinionated on what features they expose and allow to be configured
by users.

For this part of the workshop, we're going to add some more options to node using
the marvellous [cowsay2](https://github.com/johnnysprinkles/cowsay) module.

```
 ____
< hi >
 ----
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||
```

## Steps

 - [1 - Add `cowsay2` as a dependency](#51---add-cowsay2-as-a-dependency)
 - [2 - Add a cowsay option to the node](#52---add-a-cowsay-option-to-the-node)
 - [3 - Add some more cowsay options](#53---add-some-more-cowsay-options)
 - [4 - Making the edit dialog more dynamic](#54---making-the-edit-dialog-more-dynamic)

## 5.1 - Add `cowsay2` as a dependency

1. Install `cowsay2` as a regular dependency of your module:

        npm install cowsay2

## 5.2 - Add a 'cowsay' option to the node

For now, we'll simply add this as a third option for the `mode` property of the node.

1. In the `node.html` file, update the `<select>` to have a third option:

    ```html
    <select id="node-input-mode">
        <option value="lc">Lower-Case</option>
        <option value="uc">Upper-Case</option>
        <option value="cow">Cowsay</option>
    </select>
    ```

2. In the `node.js` file, load the module by adding the following to the top of
   the file:

   ```javascript
   const cowsay = require('cowsay2')
   ```

3. Inside the node's input event handler, update it to handle the new `cow` mode:

    ```javascript
    node.on('input', function(msg) {
        const strPayload = ('' + msg.payload)
        if (node.mode === 'lc') {
            msg.payload = strPayload.toLowerCase()
        } else if (node.mode === 'uc') {
            msg.payload = strPayload.toUpperCase()
        } else if (node.mode === 'cow') {
            msg.payload = cowsay.say(strPayload)
        }
        node.send(msg);
    })
    ```

4. Reload Node-RED and verify the new mode is working.

## 5.3 - Add some more cowsay options

The `cowsay2` module comes with alternative images to use in the output,.

For example, the whale:

```
 __________
< hi there >
 ----------
   \
    \
     \
                '-.
      .---._     \ .--'
    /       `-..__)  ,-'
   |    0           /
    --.__,   .__.,`
     `-.___'._\_.'
```

The next task is to provide some more options in the node to let the user
pick which image to use.

1. Add a new property to the node to hold the image choice. Update the `defaults`
   property in the `node.html` file:

    ```javascript
    defaults: {
        name: { value: "" },
        mode: { value: "lc" },
        image: { value: "cow" }
    },
    ```

2. Update the edit template to include another select box for this option:

    ```html
    <div class="form-row">
        <label for="node-input-image"><i class="fa fa-image"></i> Image</label>
        <select id="node-input-image">
            <option value="cow">Cow</option>
            <option value="whale">Whale</option>
            <option value="R2-D2">R2-D2</option>
            <option value="toaster">toaster</option>            
        </select>
    </div>
    ```

    The full list of available images is available [here](https://github.com/johnnysprinkles/cowsay/tree/master/cows). Add whatever you want!

3. Update the runtime logic to use this new property. First, add a `require`
   to load the available images:
   
   ```javascript
   const cowsayImages = require('cowsay2/cows')
   ```
   
   Make sure to store the config value on the node:

    ```javascript
    this.mode = config.mode || 'lc'
    this.image = config.iage || 'cow'
    ```

    Update the input event handler:

    ```javascript
    } else if (node.mode === 'cow') {
        msg.payload = cowsay.say(strPayload, { 
            cow: cowsayImages[node.image]
        })
    }
    ```

4. Restart Node-RED and check you can pick different images.

Note that the multi-line output of the cowsay module may end up looking a bit
odd in the Debug sidebar. But if you click on it, the output should get
properly formatted.

## 5.4 - Making the edit dialog more dynamic

Having added the `image` option in the edit dialog we now have an option
that is only relevant if the `mode` is set to `cow`. Otherwise, the image option
doesn't do anything.

From a usability point of view, it would be better if the image row was hidden,
or at least disabled, if the mode is not set to `cow`.

Node-RED lets you provide a function that should be called whenever the edit dialog
is being prepared. In this function you can setup whatever custom behaviour you
want inside the edit dialog. In this instance, we're going to add a change listener
on the `mode` select, and only show the `image` row if the mode is set to `cow`.

1. In `node.html`, within the object passed to `RED.nodes.registerType` (the one
   that has the `defaults` object in), add a property called `oneditprepare`:

   ```javascript
   defaults: {
        ...
    },
    oneditprepare: function() {
        // Add a listener on the 'change' event
        // on thw mode `<select>` element
        $('#node-input-mode').on('change', function() {
            // When changed, get the new value
            const mode = $(this).val()
            // Toggle the visibility of the
            // `node-row-image` element based on
            // its value.
            $('#node-row-image').toggle(mode === 'cow')
        })
    },
    ```

    This code will toggle the visibility of an element with an id of `node-row-image`
    based on what mode is selected.

    For that to work, add `id="node-row-image"` to the `<div class="form-row">`
    element that contains the image select box.

    As an aside, in case you're wondering, the editor provides `jQuery` for
    general use by nodes.

2. Restart Node-RED and verify the changes.


## Summary

In this section of the workshop you have:

 - used the `cowsay2` module to add more options to the node
 - added another property and UI selection for what image to use in the new mode
 - made the edit dialog more dynamic


## Next Steps

At this point, we've covered a lot of the core concepts and mechanics for creating
a node. There are lots of possible directions you could choose to take it.

In [Part 6](../part6/README.md) we'll suggest some next steps for you to explore.

***
*Quick links :*
[Home](/README.md) - [**Part 1**](../part1/README.md) - [Part 2](../part2/README.md) - [Part 3](../part3/README.md) - [Part 4](../part4/README.md) - [Part 5](../part5/README.md) - [Part 6](../part6/README.md)