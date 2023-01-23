---
layout: page
title: SoYaams
tagline: Asset Management
description: Description
---
# Soyaams
Why Soyaams? I have another repo (that I should retire) called yaams. And I am lazy.

There are three main components in the core of soyaams
- A Relational Postgres Schema
- A restful service provided by PostGREST
- A suite of client apis

Before going any further, I have to admit to a bit of inconsistency in nameing. Soyaams has gone by various names. And while i intend on cleaning this up, here they are, along with a bit of a history lesson.

First came **Yaams**. At some point around 2018-ish, I started writing Yaams, which stands for ***Yet Another Asset Management System***. I did this partly out of frustration with the pace of development at work, and partly to scratch some itches that I had in the design space. There are still a number of repositories related to yaams, including
- yaams
- rusty-yaams
- rust-yaams-derive
- yaams-maya
- yaamsbrowser
- yaams-rest
- yaams-elm
Wow, I was a busy bee. While I barfed out a lot with this first version of yaams, about the only lasting bit of interest is the fact that I discovered LTREE at that time. Other than that, it was an exercise that was well intended but not particularly great schema-wise. And i dropped it. So we move on.

Next came **SoYaams** - the ***Son of Yet Another Asset Management System***. This time, I once again was motivated to scratch an itch, revising yaams with yaams v2. I probably should have simply retired the previous repos and kept the yaams name, but I didn't.

Lastly, at some point, I got tired writing out soyaams and decided on **Lams** - the ***Last Asset Management System***. Of course, that is quite a bold thing to name it. Truthfully, I really just mean that it is my last asset management system.

## SoYaams / Lams - Core Repositories
There are a couple of main repositories for the asset management system, and a wealth of auxiliary repositories that house packages which leverage soyaams.

### soyaams
The core system is implemented as a SQL Schema for Postgres. Why Postgres? It has a number of extensions that are quite useful, including LTREE for storing hierarchical labels, and json/bson for storing json structured fields. 

The core soyaams implementation lives here in git: [soyaams repo](https://github.com/jlgerber/soyaams)

### soyaams-pyapi
There is a companion project -[`soyaams-pyapi`](https://github.com/jlgerber/soyaams-pyapi) - that provides a python api that has been hand rolled, along with a large number of command line scripts to perform crud operations. The project is set up with rez, and is compatible with python-3.7+<4.

# Additional Asset Management Repositories
In addition to the main repository defining the core soyaams asset management system, there are a wealth of additional repositories, defining 

## UIs

### Web Tech Based Asset Browser 
The [`lams-ui-sveltekit-tauri.git`](https://github.com/jlgerber/lams-ui-sveltekit-tauri) project (dubbed last asset management system by now) is a desktop soyaams browser based on web tech using tauri, rustm and svelte

### Pyside2 Asset Browser
The main asset browser -[`soyaams-qt-browser.git`](https://github.com/jlgerber/soyaams_qt_browser) - has been written to work stanalone, or in houdini (or any dcc, but houdini has been tested). This is also a rez package

## The Action Registry
While not a core part of asset management, the Action Registry - [`action_registry.git`](https://github.com/jlgerber/action_registry) - is a core part of the soyaams browser and a novel component in and of itself. It is a plugin based registry for actions. An action is a named operation on data that may have any number of implementations specific to particular data sources, dccs, snapshot types, departments, etc. The registry keeps track of actions using a runtime instance of sqlite, along with a schema and plugin system. It is a dependency of the soyaams-qt-browser.

## Action Ecosystem

There are a number of packages which provide plugins to show off the action registry's capabilities, as well as the browser.

### Action Utils
The [`action_utils.git`](https://github.com/jlgerber/action_utils) repo provies a couple of general actions designed to print snapshot information and show off specialization. 

### Action State Management
The [`action_state_management.git`](https://github.com/jlgerber/action_state_management) repo provides a number of general actions that allow one to manage currency, sealed, and ready states. The package also provides an example of a specialized widget for an action.

### Lams Otls
The [`lams_otls.git`](https://github.com/jlgerber/lams_otls) repository provides a package which houses houdini otls, actions, and widgets to load snapshots