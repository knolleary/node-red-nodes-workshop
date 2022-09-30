*Quick links :*
[Home](/README.md) - [**Part 1**](../part1/README.md) - [Part 2](../part2/README.md) - [Part 3](../part3/README.md) - [Part 4](../part4/README.md) - [Part 5](../part5/README.md) - [Part 6](../part6/README.md)
***

# Part 6:  Next Steps

**Goal:** *Some suggestions on where to take your node next*

## Introduction

This workshop has covered a lot of the foundational steps needed to create a
Node-RED node.

Of course there is a lot more detail that could be explored, but at this point,
you have the basic tools needed to create your own nodes.

This part of the workshop highlights some of the other things that you could choose
to explore next.

 - [Module Naming](#module-naming)
 - [Node Appearance](#node-appearance)
 - [Handling credential type properties](#handling-credential-type-properties)
 - [Node Edit dialog](#node-edit-dialog)
 - [Node Help](#node-help)


### Module Naming

If you've been following the steps closely, you will have a module called `workshop-node`.
This isn't suitable for publishing to the public npm catalog - so please don't!

You will see in the [Node-RED Flow Library](https://flows.nodered.org) that many
of the published nodes use the naming convention `node-red-contrib-....`. This was
our preferred approach in the early days of the project to distinguish between modules
maintained by the core project and those created by the wider community.

Our current naming guidelines are available [here](https://nodered.org/docs/creating-nodes/packaging).

In summary, we recommend modules to use a scoped name on npm.

Those docs also cover the other packaging requirements and how to get a published
node listed on the Node-RED Flow Library.

### Node Appearance

There are a few parts of your node's appearance that can be changed - such as the
icon or colour.

These are covered in the [Node Appearance](https://nodered.org/docs/creating-nodes/appearance)
section of the docs.

### Handling credential type properties

If you node needs to handle credential-type information, such as passwords, Node-RED
provides a specific way to do that securely. When properties are identified as
password-type credentials, they never get returned to the editor so they cannot be
accidentally leaked.

See [Node Credentials](https://nodered.org/docs/creating-nodes/credentials) for more
information.

### Node edit dialog

We covered some very simple cases for the node's edit dialog. The `oneditprepare`
function we introduced in the last part gives you a lot of power to create custom
dialogs.

Node-RED also provides a set of common UI widgets to help provide a consistent
user experience.

See [Node Edit Dialog](https://nodered.org/docs/creating-nodes/edit-dialog) for
more information.

### Node Help

Something we haven't really covered is the Help Text you can provide for the node.

The HTML you created had a very minimal `<script>` section for your node.

Node-RED provides a [style guide](https://nodered.org/docs/creating-nodes/help-style-guide) for writing good node help.


## Next Steps

At this point, it's over to you. You could continue to add more features to the
node we've created in this workshop, you use what you've learnt to build a node
wrapping your own favourite npm module.

***
*Quick links :*
[Home](/README.md) - [**Part 1**](../part1/README.md) - [Part 2](../part2/README.md) - [Part 3](../part3/README.md) - [Part 4](../part4/README.md) - [Part 5](../part5/README.md) - [Part 6](../part6/README.md)