# install Mozwire

Visit the following link and install Mozwire: https://github.com/NilsIrl/MozWire/


# Using Mozwire
## Create a token

Paste the following into your terminal:

```Shell
# Save a token so you don't need to authenticate on each command
export MOZ_TOKEN=$(mozwire --print-token)
```

## List servers

Use the following commands to provide a list of servers available:

```Shell
mozwire relay list
```

Make a note of a server you wish to connect to. For example `us-nyc-wg-301` or `gb-lon-wg-001` for New York and London respectively

## Create a config file

With a server noted, type the following command and paste in your respective server:

```Shell
mozwire relay save [server_name]
```

For example to connect to `gb-lon-wg-001`, use the following command:

```Shell
mozwire relay save gb-lon-wg-001
```

Make a note of the location where the config file is saved.
## Upload the config file to your GUI network settings

In your respective GUI settings, go to network settings and select VPN settings. Here, click the settings to upload the configuration files as a VPN.