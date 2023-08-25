# Nagios plugin to monitor Threshold Network tBTC nodes

tBTC nodes listen by default on port 9601 for monitoring requests to the 
`/metrics` and `/diagnostics` URIs. A version exists which will determine the operator
address and check it for sufficient ETH.

## Structure

- **`check_tbtc`** - *The plugin script. Note that though written in python the `.py` extension is not used for nagios plugins. I forget why, maybe due to parsing.*

- **`tbtcnode.cfg`** - *nagios configuration file for each node to be monitored.*

- **`threshold_logo_75x75.png`** - *Threshold logo nagios icon_image*

- **`nagios.cfg`** -  *A line which must be added to the extant `nagios.cfg` to activate the plugin, as mentioned below.*

- **`command.cfg`** - *Configuration lines which must be added to the extant `commands.cfg` to run the plugin, mentioned below.*



## Installation and Configuration Requirements

Ensure the [Class library for writing Nagios (Icinga) plugins](https://pypi.org/project/nagiosplugin/) are installed, and available to the nagios installation:

```bash
> pip3 install -U nagiosplugin
```
*-or maybe-*

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



## Configuration

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


## See if it works. If not, fix:

```bash
> nagios -v <path-to-nagios.cfg>
```



## For Manual usage (*from within .../nagios/libexec/*) :

```bash
> ./check_tbtc_node <ipaddress|FQDN> 
```


```bash
> ./check_tbtc_node -h
usage: check_tbtc_node [-h] [-p PORT] [-pw RANGE] [-pc RANGE] [-bw RANGE] [-bc RANGE] [-v] nodeip
tBTCv2 node check

positional arguments:
  nodeip                node <ipaddr>

options:
  -h, --help            show this help message and exit
  -p PORT, --port PORT  metrics/diagnostics port, default 9601
  -o OWNER, --owner OWNER
                        friendly name for a known node
  -pw RANGE, --peercountwarning RANGE
                        warning if peer count is outside RANGE
  -pc RANGE, --peercountcritical RANGE
                        critical if peer count is outside RANGE
  -bw RANGE, --bootpeerwarning RANGE
                        warning if boot peer count is outside RANGE
  -bc RANGE, --bootpeercritical RANGE
                        critical if boot peer count is outside RANGE
  -v, --verbose         increase output verbosity (use up to 3 times)
```

## Example output: 
```bash
./check_tbtc xyz.abc
CHECKNODE OK - ALL CHECKS PASSED
self.probe: checking node: xyz.abc:9601
| bootpeers=7;@~:0;@~:0 btc_connectivity=1;@~:0;@~:0 eth_connectivity=1;@~:0;@~:0 peers=141;@~:5;@~:1 pp_files=1000;1000:;1000:
```


