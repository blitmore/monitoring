# Nagios plugin to monitor Threshold Network tBTC nodes

tBTC nodes listen by default on port 9601 for monitoring requests to the 
`/metrics` and `/diagnostics` URIs. 

## Structure

- `check_tbtc`: the python plugin script. Note the `.py` extension is not used for nagios plugins, maybe 
   due to parsing. 

- `tbtcnode.cfg`: nagios configuration file for each node to be monitored.

- `threshold_logo_75x75.png`: nagios icon_image

- `nagios.cfg`: Contains a configuration line which must be added to the extant `nagios.cfg` to 
   activate the plugin. 

- `command.cfg`: Contains a configuration line which must be added to the extant `commands.cfg` to 
   run the plugin. 



## Installation and Configuration Requirements
Ensure the [Class library for writing Nagios (Icinga) plugins](https://pypi.org/project/nagiosplugin/) are installed, and available to the nagios installation:

```bash
> pip3 install -U nagiosplugin
```
-or maybe-
```bash
> pip install -U nagiosplugin
```


With appropriate privileges, 
copy the plugin itself, `check_tbtc_node`, to the nagios/libexec directory, 
alongside the other check_xyz utilities and ensure it is set executable:
```bash
> cp ./check_tbtc_node <path-to-libexec> && chmod +x <path-to-libexec>/check_tbtc_node

```

Copy the plugin definition file, `tbtcnode.cfg`, to the ../nagios/etc/objects directory, 
alongside the other cfg files: 
```bash
> cp ./tbtcnode.cfg <path-to-libexec> 
```


Copy the icon image file, `threshold_logo_75x75.png`, to the ../nagios/share/images/logos directory
```bash
> cp ./threshold_logo_75x75.png <path-to-nagios>/share/images/logos/
```



### Configuration
Edit `..nagios/etc/objects/commands.cfg` and add the following commentary + definition: 
```bash
# 'check_tbtc_node' command definition
define command{
	command_name	check_tbtc_node
	command_line	$USER1$/check_tbtc_node $HOSTADDRESS$
	}
```


Edit the tbtcnode.cfg file to set elements like these accordingly:
```
    host_name               my_tBTC_node
    alias                   My rockin Threshold tBTC node
    address                 x.y.z.b 
      .                       .
      .                       .
      .                       .
```


## Mic Check:
```bash
> nagios -v <path-to-nagios.cfg>
```



#### Manual usage (from within .../nagios/libexec/) :

```bash
  ./check_tbtc_node <ipaddress|FQDN> \
```



