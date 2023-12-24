---
layout: post
title:  "[Linux] SSH Key Based Connection"
date:   2023-12-24 14:57:25 -0500
categories: linux ssh
---
# SSH Key-Based Connection Setup Guide for Debian Linux

Based on [Debian's official SSH documentation](https://wiki.debian.org/SSH), this guide outlines the steps to set up a key-based SSH connection between two Debian Linux machines.

## On the Source Machine

### Generate SSH Key Pair

1. Open a terminal.
2. Generate an SSH key pair:
   ```shell
   ssh-keygen -t rsa
   ```
   - Follow the prompts to complete the key generation.

### Copy the Public Key to the Target Machine

1. Transfer the public key:
   ```shell
   ssh-copy-id -i ~/.ssh/id_rsa.pub user@target-machine
   ```
   - Replace `user` and `target-machine` appropriately.

## On the Target Machine

### Verify SSH Key

1. Check `~/.ssh/authorized_keys`.

### Configure SSH Daemon (Optional)

1. Edit SSH configuration:
   ```shell
   nano /etc/ssh/sshd_config
   ```
2. Restart SSH service:
   ```shell
   service ssh restart
   ```

## Testing the Connection

- Connect to the target machine:
  ```shell
  ssh user@target-machine
  ```

## Troubleshooting

- **Permissions**: As advised in Debian SSH security practices, set strict permissions for SSH directories:
  ```shell
  chmod 700 ~/.ssh
  ```

  ```shell
  chmod 600 ~/.ssh/authorized_keys
  ```
- **Firewall Settings**: Ensure the firewall on the target allows SSH connections on port 22, consistent with Debian standards.
- **SSH Service**: Verify that the SSH service is active on the target machine, as per Debian's guidelines.

For more details, see the [Debian SSH Wiki](https://wiki.debian.org/SSH).
