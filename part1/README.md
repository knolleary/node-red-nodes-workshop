*Quick links :*
[Home](/README.md) - [**Part 1**](../part1/README.md) - [Part 2](../part2/README.md) - [Part 3](../part3/README.md) - [Part 4](../part4/README.md) - [Part 5](../part5/README.md) - [Part 6](../part6/README.md)
***

# Part 1: Getting Started with Node-RED

**Goal:** *Install Node-RED locally and get your local development environment setup*

## Introduction

This section of the workshop gets you setup with Node-RED running locally and a
local development environment ready for creating your nodes.

## Steps

 - [1 - Install Node-RED](#11---install-node-red)
 - [2 - Create some flows](#12---create-some-flows)
 - [3 - Setup a development environment](#13---setup-a-development-environment)
 - [4 - Creating the module](#14---creating-the-module)
 - [5 - Install nodemon](#15---install-nodemon)
 - [6 - Running Node-RED with nodemon](#16---running-node-red-with-nodemon)
 
## 1.1 - Install Node-RED

Node-RED can be installed by running:

    npm install -g node-red

Once installed, it can be started by using the command `node-red`.

The log output will look similar to:

```
$ node-red

27 Sep 20:49:30 - [info]

Welcome to Node-RED
===================

27 Sep 20:49:30 - [info] Node-RED version: v3.1.0-beta.0
27 Sep 20:49:30 - [info] Node.js  version: v16.13.1
27 Sep 20:49:30 - [info] Darwin 20.5.0 x64 LE
27 Sep 20:49:30 - [info] Loading palette nodes
27 Sep 20:49:31 - [info] Dashboard version 2.30.0 started at /ui
27 Sep 20:49:31 - [info] Settings file  : /Users/nol/.node-red/settings.js
27 Sep 20:49:31 - [info] Context store  : 'default' [module=memory]
27 Sep 20:49:31 - [info] Context store  : 'other' [module=memory]
27 Sep 20:49:31 - [info] User directory : /Users/nol/.node-red
27 Sep 20:49:31 - [info] Projects directory: /Users/nol/.node-red/projects
27 Sep 20:49:31 - [info] Server now running at http://127.0.0.1:1880/
```

Two things to make a note of from this output:
 - the **Node-RED Settings File** location, typically `~/.node-red/settings.js`
 - the **Node-RED User Directory** location, typically `~/.node-red`

You will need to refer back to these locations through-out this workshop.

Once running, you can open the url it provides in your web browser to access the
Node-RED editor.

## 1.2 - Create some flows

Before you get started creating your own node, it's worth spending some time
playing with Node-RED to better understand what it's all about.

If you're already familiar with Node-RED, you can skip this step.

The Node-RED documentation has a pair of tutorials to help get you started:

 - [Your first flow](https://nodered.org/docs/tutorials/first-flow)
 - [Your second flow](https://nodered.org/docs/tutorials/second-flow)

## 1.3 - Setup a development environment

When Node-RED starts, it scans a number of locations searching for modules
that contain nodes.

For this workshop you'll be developing a node, so need make sure you're doing that
somewhere Node-RED can find it.

1. Under your Node-RED User Directory (`~/.node-red`), create a directory called `nodes`

2. Under that directory, create a directory for your node - call it `workshop-node` for now.

```
~/.node-red
 └── nodes
     └── workshop-node
```


## 1.4 - Creating the module

Node-RED nodes are packaged as CJS modules - ES6 modules are not directly supported.

1. Add a `package.json` file in your `workshop-node` directory. The quick way of doing this
   is to run `npm init` in the directory and follow the prompts. What values you
   choose are less important at this stage - we'll get into specific requirements
   around the module name in the next section. For now, we just need a `package.json`
   to exist.


## 1.5 - Install nodemon

When developing a node, you have to restart Node-RED to pickup any changes you
have made.

To save from doing that manually, you can use `nodemon` to watch your working
directory and restart Node-RED whenever files change.

1. Within the `workshop-node` directory, install `nodemon` and add it as a devDependency:

        npm install --save-dev nodemon

## 1.6 - Running Node-RED with nodemon

You can now run Node-RED with `nodemon` monitoring your `workshop-node` directory for changes.

1. If Node-RED is already running from step 1.1, stop it now.
2. Then, from within the `workshop-node` directory, run:

        npx nodemon -w . -e 'js,html,json' node-red


Note this assumes `node-red` is on your path. If it isn't, you'll need to specify the
full path to the `node-red` command installed with the global module in step 1.1.

You can test its working by creating a temporary file called `test.js` in the `workshop-node` directory - you
should see Node-RED get restarted.

## Summary

In this section of the workshop you have:

 - Installed Node-RED
 - Setup a working directory for your node
 - Installed nodemon and used it to run Node-RED whilst monitoring for file changes

## Next Steps

The next task is to create the initial node. To do that, we need to create
some more files in your `workshop-node` directory in [Part 2](../part2/README.md).


***
*Quick links :*
[Home](/README.md) - [**Part 1**](../part1/README.md) - [Part 2](../part2/README.md) - [Part 3](../part3/README.md) - [Part 4](../part4/README.md) - [Part 5](../part5/README.md) - [Part 6](../part6/README.md)