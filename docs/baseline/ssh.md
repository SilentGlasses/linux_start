---
tags:
  - ssh keys
---

# Using SSH Keys

!!! danger "Important"-
    - **Passphrase Requirement**:
        - A passphrase must be used when creating SSH keys. Using a passphrase increases the security when you are using SSH keys. Using a key without a passphrase can be risky.
        - If someone obtains a key (from a backup tape, or a one-time vulnerability) that doesn't include a passphrase, the remote account can be compromised.
    - Your SSH keys belong to you, not your computer. This means if you get a new computer, copy your existing keypair(s) over to the new one.
    - **NEVER** share your private key. **Always** make sure you are ever only using the key found in your `<keyname>.pub` file.

## Generating keys

Create your PEM formatted key(s) then add it/them to your keychain and wherever you will need them.

Run the following command to initiate the creation:

```
ssh-keygen -m PEM -t rsa -b 4096 -f ~/.ssh/id_rsa -C "$(whoami)@$(uname -n)_$(date -I)"
ssh-keygen -m PEM -t ed25519 -f ~/.ssh/id_ed25519 -C "$(whoami)@$(uname -n)-$(date -I)"
```

Now you need to enter a passphrase.

```
# Enter passphrase (empty for no passphrase): [Type a passphrase]
# Enter same passphrase again: [Type passphrase again]
```

Which should give output like this:

```
Your identification has been saved in /Users/username/.ssh/id_rsa.
Your public key has been saved in /Users/username/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:vpEQnYp+5w9qbBznyb2r+Y9GwoHJBrExTOXgelISgLo username@computer_YYYY-MM-DD
The key's randomart image is:
+---[ECDSA 521]---+
|o..o*o.          |
|.  oo* . .       |
|. . +oooo        |
|.  + .=o.        |
| .o o.o.S.       |
|E  +  .o+..      |
|    .o.=*=       |
|     .=++=o.     |
|     o. ==*+.    |
+----[SHA256]-----+
```

## Secure File Permissions

Ensure your ssh directory and key files are properly secured:

- for the directory
```
chmod 700 ~/.ssh
```
- for private keys
```
chmod 600 ~/.ssh/id_rsa
chmod 600 ~/.ssh/id_ed25519
```
- for public keys
```
chmod 644 ~/.ssh/id_rsa.pub
chmod 644 ~/.ssh/id_ed25519.pub
```

!!! note
    - `chmod 700` or `drwx------`: Grants all permissions only to the owner, and removes all other permissions to everyone else.
    - `chmod 600` or `-rw-------`: Grants read and write permissions to the owner, and removes all other permissions to everyone else.
    - `chmod 644` or `-rw-r--r--`: Grants read and write permissions to the owner, and read-only permissions to everyone else.

## Adding your Key to the SSH Agent

- Start the SSH Agent in the background
```
eval "$(ssh-agent -s)"
```
- Add the key:
```
ssh-add ~/.ssh/id_rsa
ssh-add ~/.ssh/id_ed25519
```
- Setup your ssh config:
```
vi ~/.ssh/config
```
- Paste in the following at the top:
```
Host *
	AddKeysToAgent yes
	IdentityFile ~/.ssh/id_ed25519
```
- List all managed keys:
```
ssh-add -l
```

## Using Keychain

- Install Keychain:
```
sudo apt install keychain
```
- Add this to your `.zshrc` file:
```
/usr/bin/keychain $HOME/.ssh/id_ed25519
source $HOME/.keychain/$HOSTNAME -sh
```
