# AD-FreeIPA Synchronization Script

This script is designed to synchronize user groups between an Active Directory (AD) and a FreeIPA server. It performs several key functions:

## Features

### AD and FreeIPA Connection
- Establishes connections to both AD and FreeIPA servers using provided credentials and server details.

### Group and User Synchronization
- Retrieves groups and their users from AD and FreeIPA.
- Identifies new groups in AD that are not in FreeIPA and adds them.
- Finds and removes groups in FreeIPA that no longer exist in AD.
- Synchronizes users within these groups between AD and FreeIPA.

### Logging
- Configures logging to record activities and errors, both to a file and to the console.

### Configuration
- Uses a `.conf` file to load necessary configurations like server addresses, credentials, and logging settings.

## Key Functions
- `ad_connect`: Connects to the AD server using LDAP over SSL.
- `connect_to_freeipa`: Connects to the FreeIPA server.
- `ad_search_groups_with_users`: Searches for groups in AD and lists their users.
- `ipa_search_groups_with_users`: Searches for external groups in FreeIPA and lists their users.
- `ipa_add_groups` and `ipa_del_groups`: Adds and deletes groups in FreeIPA.
- `ipa_add_del_users_in_groups`: Synchronizes users in groups between AD and FreeIPA.

## Configuration File (`ad_ipa.conf`)
- Contains settings for AD and FreeIPA server connections, including server addresses, user credentials, and certificate paths.
- Specifies logging configurations like log file name, level, format, and encoding.

## Usage

### Setup
Ensure that the `ad_ipa.conf` file is correctly configured with the necessary server details and credentials.

### Execution
Run the script. It will read the configuration, establish connections to AD and FreeIPA, and perform synchronization based on the current state of groups and users in both systems.

### Logging
Monitor the `ad_ipa.log` file for detailed logs of the script's operations and any issues encountered.

## Important Notes

### Security
The script handles sensitive information (like credentials). Ensure it's run in a secure environment and the configuration file is protected.

### Error Handling
The script exits if it cannot connect to all AD or FreeIPA servers. Ensure network stability and correct server details.

### AD and FreeIPA Versions
The script's compatibility depends on the versions of AD and FreeIPA. Ensure they are compatible with the used libraries (`ldap3`, `python_freeipa`).

### SSL Certificates
The script uses SSL for connections. Ensure the correct SSL certificates are provided and valid.

### Python Environment
Requires Python 3 and specific libraries (`ldap3`, `python_freeipa`, `dotenv`). Ensure these are installed in your Python environment.


To install and set up the AD-FreeIPA synchronization script, follow these steps:

1. **Create a Directory and Clone the Repository:**
   ```bash
   mkdir /opt/ad_ipa_sync
   cd /opt/ad_ipa_sync
   git clone https://github.com/catkaff/ad_freeipa_sync.git

2. **Set Up Python Environment**
   ```bash
   python3.9 -m venv env
   source env/bin/activate
   pip install -r ad_freeipa_sync/requirements.txt
   deactivate

3. **Set Up Systemd Service and Timer**
   ```bash
   cp /opt/ad_ipa_sync/ad_freeipa_sync/ad_ipa_sync.service /etc/systemd/system/
   cp /opt/ad_ipa_sync/ad_freeipa_sync/ad_ipa_sync.timer /etc/systemd/system/
   systemctl daemon-reload
   systemctl enable --now ad_ipa_sync.timer



