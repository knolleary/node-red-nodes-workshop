*Quick links :*
[Home](/README.md) - [Part 1](../part1/README.md) - [Part 2](../part2/README.md) - [Part 3](../part3/README.md) - [**Part 4**](../part4/README.md) - [Part 5](../part5/README.md) - [Part 6](../part6/README.md)
***

# Part 4: Writing unit tests for your node

**Goal:** *Use Node-RED Node Test Helper to create unit tests for your node*

## Introduction

Before you starting more features to the node, we want to get some tests in
place for it.

To make life easier, Node-RED provides a framework that lets you write
individual tests that load a flow, inject messages and examine the results.


## Steps

 - [1 - Setup test dependencies](#41---setup-test-dependencies)
 - [2 - Create your first test](#42---create-your-first-test)
 - [3 - Add some more useful tests](#43---add-some-more-useful-tests)

## 4.1 - Setup test dependencies

Before we can write tests, we need to add some modules to the node's `devDependencies`.

For this workshop, we're using `mocha` and `should` as our preferred test runner
and assertion library. You are welcome to use others, but this is what we're
using here.

1. In your `workshop-node` directory, run the following command to install the required
   modules:

   ```
   npm install --save-dev node-red node-red-node-test-helper mocha should
   ```

## 4.2 - Create your first test

1. Create a file called `test/node_spec.js` with the following content:


    ```javascript
    const should = require("should");
    const helper = require("node-red-node-test-helper");
    const workshopNode = require("../node.js");

    // Initialise the test helper with the location of node-red.
    // This means it is possible to avoid adding `node-red` as an
    // explicit devDependency if you know it has been installed
    // separately.
    helper.init(require.resolve('node-red'));

    describe('Workshop Node', function () {
        
        // Before each test we have to start the helper
        beforeEach(function (done) {
            helper.startServer(done)
        });
        
        // After each test we have to stop the helper
        afterEach(function (done) {
            helper.unload()
            helper.stopServer(done)
        });
        
        // A minimal test that verifies the node gets loaded properly into
        // the runtime
        it('should be loaded', function (done) {
            // Each test provides a flow that should be tested.
            // For this test, we have a single instance of our workshop-node
            const flow = [
                { id: "node1", type: "workshop-node", name: "test-node-instance", mode: "lc" }
            ]

            // Tell the helper to load the node and test flow
            helper.load(workshopNode, flow, function () {
                // At this point, the helper will have started
                // the flow running.
                // Get ahold of the node instance
                const node1 = helper.getNode("node1")
                try {
                    // Validate it exists and has the expected name property
                    node1.should.have.property('name', 'test-node-instance')
                    done()
                } catch(err) {
                    done(err)
                }
            })
        })
    })
    ```

2. To run this test, in your `workshop-node` directory, run:

        npx mocha test/node_spec.js

   All being well, the test should pass.
   ```
    Workshop Node
        ✔ should be loaded


    1 passing (93ms)
    ```

## 4.3 - Add some more useful tests

The current test just validates the node loads without error. Now we should add
some tests for the functionality the node provides.

1. Add the following test to `node_spec.js`. The inline comments explain what
   it's doing.

    ```javascript
    it('should convert payload to lower-case', function (done) {
        // This test includes a helper node in the flow to capture
        // the output of the node under test.
        // Note the 'wires' property used to identify what the
        // test node is connected to.
        const flow = [
            {
                id: "node1",
                type: "workshop-node",
                name: "test-node-instance",
                mode: "lc",
                wires: [[ "outputNode" ]]
            },
            { id: "outputNode", type: "helper" }
        ]

        // Tell the helper to load the node and test flow
        helper.load(workshopNode, flow, function () {
            const node1 = helper.getNode("node1")
            const outputNode = helper.getNode("outputNode")

            // Add a listener on the outputNode's 'input' event
            outputNode.on("input", function (msg) {
                try {
                    // Validate the payload has been lower-cased
                    msg.should.have.property('payload', 'abcde')
                    done()
                } catch(err) {
                    done(err)
                }
            })

            // Inject a message into our test node
            node1.receive({ payload: "aBcDe" })
        })
    })
    ```

2. Add another test to verify the behaviour of the node when in 'upper-case' mode. You aren't getting the code to copy and paste this time, but here are some hints:
    - use the lower-case test as a starting point
    - configure the node with `mode` set to `uc`
    - test the result is upper-case text

3. Make sure your tests are passing.

## Summary

In this section of the workshop you have:

 - added some tests for your node


## Next Steps

At this point, we've covered a lot of the core concepts and mechanics for creating
a node. The next task is to show how a node can be used to wrap an existing npm
module, building on what you've already done. Head to [Part 5](../part5/README.md)
to continue the adventure.

***
*Quick links :*
[Home](/README.md) - [Part 1](../part1/README.md) - [Part 2](../part2/README.md) - [Part 3](../part3/README.md) - [**Part 4**](../part4/README.md) - [Part 5](../part5/README.md) - [Part 6](../part6/README.md)