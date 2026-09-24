# Day 04 — Introduction to the Cisco IOS CLI

## Overview

Day 4 focused on the **Cisco IOS CLI** and how to access and configure Cisco devices using the command line. I learned the different IOS modes, basic CLI navigation, password configuration, password encryption, configuration files, and how to save configurations.

I also practiced these concepts in Cisco Packet Tracer by configuring a router and switch with basic device security.

## What I Learned

- The **CLI (Command-Line Interface)** is used to configure Cisco devices.
- Cisco devices can be accessed through the **console port** using an RJ45 rollover cable or USB console connection.
- **User EXEC mode** (`Router>`) provides limited access.
- **Privileged EXEC mode** (`Router#`) provides access to more device information and commands.
- **Global configuration mode** (`Router(config)#`) is used to make configuration changes.
- The `?` key can be used to view available commands.
- The `Tab` key can complete commands when the entered characters are sufficient to identify the command.
- Cisco IOS has two configuration files:
  - **running-config** — the current active configuration.
  - **startup-config** — the configuration loaded when the device restarts.
- `enable password` provides a plain-text privileged EXEC password.
- `service password-encryption` encrypts current and future passwords in the configuration using Type 7.
- `enable secret` provides an automatically encrypted privileged EXEC password and uses Type 5 (MD5) in the course material.
- Configurations must be saved to the startup configuration if they should remain after a restart.

## Key Concepts

| Concept | What I Learned |
|---|---|
| User EXEC | `Router>` — limited access |
| Privileged EXEC | `Router#` — higher-level access |
| Global Configuration | `Router(config)#` — used to configure the device |
| `enable password` | Configures an unencrypted enable password |
| `service password-encryption` | Encrypts passwords displayed in the configuration |
| `enable secret` | Configures an automatically encrypted enable password |
| Type 7 | Used by `service password-encryption` |
| Type 5 / MD5 | Used by `enable secret` in the course material |
| running-config | Current active configuration |
| startup-config | Saved configuration loaded after restart |

## Packet Tracer Lab

For the Day 4 Packet Tracer lab, I configured the router and switch with basic device security.

The lab involved:

- Changing the device hostnames to `R1` and `SW1`
- Configuring the unencrypted enable password as `CCNA`
- Testing the password after returning to User EXEC mode
- Viewing the password in the running configuration
- Enabling `service password-encryption`
- Checking the encrypted password in the running configuration
- Configuring the encrypted enable secret as `Cisco`
- Checking the configured passwords and their encryption types
- Saving the running configuration to the startup configuration

## Network Topology

The Packet Tracer topology contains one router, one switch, and three PCs:

```text
        ┌── PC1
        │
R1 ── SW1 ├── PC2
        │
        └── PC3
```

No IP addressing information was provided in the uploaded lab material, so it is not included here.

## Commands Practiced

```text
enable
```
Used to enter privileged EXEC mode.

```text
configure terminal
```
Used to enter global configuration mode.

```text
hostname R1
hostname SW1
```
Used to change the device hostname.

```text
enable password CCNA
```
Used to configure the unencrypted enable password.

```text
exit
```
Used to leave the current configuration mode.

```text
show running-config
```
Used to view the current active configuration and check the configured passwords.

```text
service password-encryption
```
Used to encrypt current and future passwords displayed in the configuration.

```text
enable secret Cisco
```
Used to configure an automatically encrypted enable password.

```text
no service password-encryption
```
Used in the course practice to remove the `service password-encryption` command.

```text
show startup-config
```
Used to view the saved configuration.

```text
write
write memory
copy running-config startup-config
```
Used to save the running configuration to the startup configuration.

## Key Takeaways

- I understood the difference between **User EXEC, Privileged EXEC, and Global Configuration modes**.
- I learned how to move between IOS modes using commands such as `enable` and `configure terminal`.
- I understood the difference between **running-config and startup-config**.
- I practiced configuring `enable password` and `enable secret`.
- I learned how `service password-encryption` affects passwords shown in the configuration.
- I understood that `enable secret` uses **Type 5 (MD5)** in the course material, while `service password-encryption` uses **Type 7**.
- I practiced saving the running configuration so it can be loaded after a restart.

## Lab Evidence

The Day 4 lab folder contains the Packet Tracer files and separate router/switch configuration evidence.

### Packet Tracer Files

- `Day+04+Lab+Arpit+Solution.pkt` — My completed Packet Tracer solution
- `Day+04+Lab+Question+Basic+Device+Secu....pkt` — Original Packet Tracer lab question

### Switch Configuration

The `Switch Configuration` folder contains screenshots documenting the configuration steps, including:

- `01_Network_Setup.png` — Initial network setup
- `02_Hostname_SW1.png` — Switch hostname configuration
- `03_Enable_Password_CCNA.png` — Enable password configuration
- `04_CCNA_Password_Works_Running_Config....png` — Running configuration/password verification
- `05_CCNA_Service_Password_Encryption_Run....png` — Service password encryption configuration
- `06_Enable_Secret_Cisco.png` — Enable secret configuration
- `07_Show_MDI5_Encryption_Secret.png` — Encryption/enable secret verification
- `08_Write_Write_Memory_Copy.png` — Saving the configuration

### Router Configuration

The `Router Configuration` folder contains screenshots documenting the router configuration steps, including:

- `01_Network_Setup.png` — Initial network setup
- `02_Hostname_R1.png` — Router hostname configuration
- `03_Unencrypted_Password_CCNA.png` — Unencrypted enable password
- `04_CCNA_Password_Works.png` — Password testing
- `05_Running_Config_Show_CCNA_Password.png` — Running configuration verification
- `06_CCNA_Encrypted_Service_Password_Encry....png` — Service password encryption
- `07_CCNA_Password_Encrypted_Running_Con....png` — Encrypted password in running configuration
- `08_Enable_Secret_Cisco.png` — Enable secret configuration
- `09_Cisco_Used_For_Password_Encryption_M....png` — Password encryption verification
- `10_Write_Write_Memory_Copy_Works.png` — Saving the configuration

## Conclusion

Day 4 gave me hands-on practice with the Cisco IOS CLI and basic device security. I learned how IOS modes work, how to configure and verify passwords, how password encryption appears in the configuration, and how to save the running configuration to startup configuration. The Packet Tracer lab helped me apply these commands on both a router and a switch.
