---
layout: page
title: Salesforce Scripting
parent: Salesforce
---


# Salesforce Scripting Configuration

There are a few python scripts that interact directly with Salesforce. Here is a non-exhaustive list:
* `cdEngine.py`
* `sfsync.py`
* `transcodeEngine.py`

These connect to salesforce via an API key. The API key must be kept private, so it's kept separately in a configuration file named `config.py`. This config file needs to be in the same directory as the script that's being run. On the SAN the config file lives in the `Scripts` folder along with the rest of the scripts.

The config file has the username/email, password, and API key for the salesforce account that the scripting uses to connect to our salesforce.

The `config.py` file does not live in the [GitHub repo](https://github.com/bavc/videomachine/). This is because the information is private and should not live in a public repo. To keep the file from being uploaded, `config.py` has been added to the `.gitignore` file in repo so that it won't accidentally be uploaded. There is a file named `config_template.py` that lives in the repo that is just an empty version of the config file. You can use this to create a new config file if you need. The path is: `Preservation/Inventory/Software/Scripting/config.py`
