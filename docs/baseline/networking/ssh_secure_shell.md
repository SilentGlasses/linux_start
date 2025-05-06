# SSH (Secure Shell)

SSH is a secure protocol for remotely managing Linux systems. It allows you to execute commands on remote servers, transfer files, and manage systems securely.

## Connecting to a Remote Machine

- **Command**:
  ```bash
  ssh user@remote-host
  ```
  - Replace `user` with your username and `remote-host` with the server's IP or domain.

### Troubleshooting Connection Issues:

1. **Authentication Failures**:
   - Ensure the correct username and password.
   - Check for missing or incorrect SSH keys.

2. **Firewall or Port Issues**:
    - Verify that port 22 (default SSH) is open on the remote machine:
    ```bash
    ss -tuln | grep 22
    ```

## Copying Files Securely

=== "scp"
    - **Command**:
    ```bash
    scp file.txt user@remote-host:/path/to/destination
    ```
=== "rsync"
    - **Command**:
    ```bash
    rsync -av file.txt user@remote-host:/path/to/destination
    ```

#### Troubleshooting Example:

- If file transfers fail, ensure `scp` or `rsync` is installed on both systems.
- Verify permissions on target directories.

## Key Management

=== "Generate SSH Keys"
    See the [SSH Keys](ssh.md) instructions on how to properly setup your SSH Keys.

    - **Command**:
    ```bash
    ssh-keygen -t rsa
    ```
=== "Copy Public Key to Remote Server"
    - **Command**:
    ```bash
    ssh-copy-id user@remote-host
    ```

#### Troubleshooting Example:

- If key authentication fails:
  - Verify the public key is present in `~/.ssh/authorized_keys` on the remote server.
  - Check permissions for `~/.ssh` (should be `700`) and `authorized_keys` (should be `600`).

By mastering SSH, you'll be equipped to manage remote systems securely and efficiently.
