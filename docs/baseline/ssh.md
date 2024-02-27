---
tags:
  - ssh keys
---

# Using SSH Keys

!!! danger "Important"
    - Your SSH keys belong to you, not your computer. This means if you get a new computer, copy your existing keypair(s) over to the new one.
    - **NEVER** share your private key. **Always** make sure you are ever only using the key found in your `<keyname>.pub` file.

## Choosing an Algorithm and Key Size

I use **ed25519**, despite it saying that it's not widely used, more and more places support it in production and more are supporting all the time.

!!! quote
    SSH supports several public key algorithms for authentication keys. These include:

    - **rsa** - an old algorithm based on the difficulty of factoring large numbers. A key size of at least 2048 bits is recommended for RSA; 4096 bits is better. RSA is getting old and significant advances are being made in factoring. Choosing a different algorithm may be advisable. It is quite possible the RSA algorithm will become practically breakable in the foreseeable future. All SSH clients support this algorithm.
    - **dsa** - an old US government Digital Signature Algorithm. It is based on the difficulty of computing discrete logarithms. A key size of 1024 would normally be used with it. DSA in its original form is no longer recommended.
    - **ecdsa** - a new Digital Signature Algorithm standarized by the US government, using elliptic curves. This is probably a good algorithm for current applications. Only three key sizes are supported: 256, 384, and 521 (sic!) bits. We would recommend always using it with 521 bits, since the keys are still small and probably more secure than the smaller keys (even though they should be safe as well). Most SSH clients now support this algorithm.
    - **ed25519** - this is a new algorithm added in OpenSSH. Support for it in clients is not yet universal. Thus its use in general purpose applications may not yet be advisable.

    ~ [Source: ssh.com](https://www.ssh.com/ssh/keygen)

### Generating keys

Create your PEM formatted key(s) then add it/them to your keychain and wherever you will need them.

Run the following command to initiate the creation:

```
ssh-keygen -m PEM -t rsa -b 4096 -f ~/.ssh/id_rsa -C "$(whoami)@$(uname -n)_$(date -I)"
ssh-keygen -m PEM -t ed25519 -f ~/.ssh/id_ed25519 -C "$(whoami)@$(uname -n)_$(date -I)"
```

Now you need to enter a passphrase.

!!! note
    - When generating keys, we **HIGHLY RECOMMEND** using a secure passphrase. Using a passphrase increases the security when you are using SSH keys. Using a key without a passphrase can be risky.
    - If someone obtains a key (from a backup tape, or a one-time vulnerability) that doesn't include a passphrase, the remote account can be compromised.

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

### Adding your Key to the SSH Agent

- Start the SSH Agent in the background
```
eval "$(ssh-agent -s)"
```
- Add the key:
```
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

- Install keychain
```
sudo apt install keychain
```
- Add the following lines to your `.bashrc` or `~/.zshrc` file:
```
# Handling SSH Keys
#=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
eval `keychain --eval id_ed25519`
```
