# wazuh-fortigate-rules-decoders

This project provides a robust set of decoders and rules designed to integrate Fortigate logs with Wazuh, developed based on the Fortigate Log Reference for versions 7.0.14, 7.2.7, 7.2.8, and 7.4.3. 

I am working hard on that to improve and expand the functionalities. Leave a star and follow this repo for updates.

## Features

- **738 Decoders**: For decoding and analyzing Fortigate logs.
- **1388 Rules**: For categorizing specific logs, facilitating monitoring and event analysis.

## Installation

1. **Copy the decoders and rules to your Wazuh Manager**

   - Copy `0100-fortigate_decoders.xml` to `/var/ossec/etc/decoders/`
   - Copy `0391-fortigate_rules.xml` to `/var/ossec/etc/rules/`

2. **Restart the Wazuh Manager**

   ```bash
   systemctl restart wazuh-manager
   ```

## Wazuh manager IOC list registration

The bundled Wazuh IOC rules `99901–99920` require three CDB lists to be registered in the manager configuration. This is separate from the FortiGate rules and decoders in this repository. On managers upgraded from Wazuh 4.12.x or earlier, the list files may exist while their configuration declarations are missing.

Verify that these files exist and contain the intended indicators. Add each reference once inside the existing `<ruleset>` block in `/var/ossec/etc/ossec.conf`, preserving the other settings:

```xml
<list>etc/lists/malicious-ioc/malware-hashes</list>
<list>etc/lists/malicious-ioc/malicious-ip</list>
<list>etc/lists/malicious-ioc/malicious-domains</list>
```

Restart the manager and start a fresh Ruleset Test session. Confirm that missing-list warnings and the dependent `99906` warning are gone. Test a known indicator from an installed list and an unlisted control; the control should not match an IOC rule. Registering the lists does not configure automatic indicator-feed updates.

See the [official Wazuh CDB-list upgrade instructions](https://documentation.wazuh.com/current/upgrade-guide/upgrading-central-components.html#configuring-cdb-lists).
