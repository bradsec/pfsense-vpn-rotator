# pfSense OpenVPN Client Rotator / Randomizer

## Overview

`pfsense-vpn-rotator.sh` is a shell script specifically designed for pfSense systems. Its primary function is to automate the process of rotating and randomizing server addresses and ports for **existing** OpenVPN client configurations. This script ensures dynamic and secure VPN connections by periodically altering the VPN endpoints.

## Features

- Safely rotates VPN server configurations using the `pfSsh.php` command.
- Eliminates the risks associated with direct editing of `config.xml`.
- Automatically selects a random VPN server from predefined lists.
- Dynamically updates OpenVPN client configurations on pfSense.
- Restarts the VPN service to apply changes seamlessly.
- Supports multiple server lists based on VPN IDs.

## Why `pfSsh.php`?

The script leverages `pfSsh.php` for configuration changes rather than directly modifying `config.xml`. This approach reduces the risk of file corruption and syntax errors, ensuring the stability and integrity of your pfSense system's configuration. It's a best practice recommended for making programmable changes to pfSense configurations.

## Prerequisites

- You must have fully configured and working OpenVPN client configurations.
- pfSense 2.7.2 or later.
- Access to the pfSense shell and `/usr/local/sbin/pfSsh.php`.
- Basic understanding of shell scripting and pfSense configuration.

## Installation

1. Download and edit the script as required.
2. Copy the script to your pfSense server (e.g., /usr/local/sbin).
3. Make the script executable: `chmod +x pfsense-vpn-rotator.sh`.
4. Ensure that your server lists (`server_list1`, `server_list2`, etc.) are correctly defined within the script. Each list should correspond to a specific VPN ID. (Example has ProtonVPN AU and US server lists).  

Note: The name variable above the server_list (`server_name1`, `server_name2`, etc.) will be added to the OpenVPN client description to make it easier to identify the VPN connections in the pfSense WebUI.

## Quick Download and Install Method

```terminal
# Run from pfSense terminal (CLI)
curl -o /usr/local/sbin/pfsense-vpn-rotator.sh https://raw.githubusercontent.com/bradsec/pfsense-vpn-rotator/main/pfsense-vpn-rotator.sh

# Make the script executable
chmod +x /usr/local/sbin/pfsense-vpn-rotator.sh

# Run script as required (example below for OpenVPN client (vpnid) 1)
/usr/local/sbin/pfsense-vpn-rotator.sh 1
```
  
## Usage

```terminal
/usr/local/sbin/pfsense-vpn-rotator.sh <vpnid>

# Replace `<vpnid>` with the appropriate VPN ID.
```

Run the script directly from the pfSense shell or use the cron package in pfSense for scheduling the script execution. To install cron from WebUI goto `System > Package Manager`. If `Cron` is not in `Installed Packages` go to `Available Packages` and search `Cron` and install. Once installed `Cron` will appear under the `Services` pfSense menu.  
  
Below is an example of Cron job running the script every 6 hours for OpenVPN client (vpnid) 1
```terminal
0 	*/6 	* 	* 	* 	root 	/usr/local/sbin/pfsense-vpn-rotator.sh 1
```

## Troubleshooting
If you are unsure of your vpnid you can run the following commands from the shell on pfSense to view the Openvpn client configuration information:
```terminal
pfSsh.php
print_r($config['openvpn']['openvpn-client']);
exec;
exit
```

## License

This script is released under the [MIT License](LICENSE).

## Disclaimer

This script is provided "as is", without warranty of any kind. Use it at your own risk. Always ensure you have backups of your configurations before running any automation scripts.
