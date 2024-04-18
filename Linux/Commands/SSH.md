SSH, or Secure Shell, is a protocol used to securely connect to remote computers over a network. It provides a secure channel for accessing and managing devices, typically using a command-line interface, though there are also graphical user interface (GUI) applications available.

# Connecting to a computer using SSH

To connect to a device using SSH, open a terminal and type the following command into it:

```Shell
ssh [username]@[ip.address.to.server]
```

This will prompt you to type in the password of the username that is on that device. Here is an example:

```Shell
ssh callum@192.168.0.2
```

You can also log into a computer using the same account that you are currently using by just omitting the username at the start, like so:

```Shell
ssh 192.168.0.2
```

If I am logged into an account called `callum`, it'll automatically try to log into the device as `callum`.

# SSH-Keygen

Another method of login via SSH is known as SSH Keys. These are a pair of public and private keys that allow authentication between two devices.
- A Public key is a key that can be shared freely without any negative consequences. The public key can be used to encrypt messages that **only** the private key can decrypt.
- A Private Key on the other hand is retained only by the client, and should not be shared under any circumstances. Since the private key can decrypt public keys, this allows any attacker with your private key to log into devices without any additional authentication.

## Setting up SSH Keys

### Create a SSH key pair

You can create a SSH key pair using the following command:

```Shell
ssh-keygen
```

You may be prompted to store the file in a specific location. If left empty, the default is: `~/.ssh`.

Your private key will be called `id_rsa`.
Your public key will be called `id-rsa.pub`.

Ideally it is best to leave the location as default due to the SSH client being able to automatically find the keys when authentication is attempted.

You may also be prompted to enter a passphrase for the key pair. This is optional, and is used to encrypt the private key on your disk.

### Copying your key to a server

The most common and simpliest way to copy your public key to a server is to use a built in SSH utility called `ssh-copy-id`.

To use it, simply enter the following command:

```Shell
ssh-copy-id [username]@[ip.address.to.server]
```

You may be asked to authenticate that you are connected to the server you wish to connect to, as well as authenticate that you are the user you say you are.

Once this has been complete, the public key will be sent to the server and stored in your home directory under `~./.ssh/authorized_keys`.

### Disabling password authentication on the server

Now that you can log into your server without the need of a password, it is suggested that you disable password authentication so that brute-force attacks can be prevented. 

> [!Warning] Warning
> Before completing the steps in this section, make sure that you either have SSH key-based authentication configured for the **root** account on this server, or preferably, that you have SSH key-based authentication configured for an account on this server with `sudo` access. If you do not do this beforehand, you will lock yourself out of administrative access on your server.

Log into your remote server with SSH keys, either as `root` or with an account with `sudo` privileges. Open the SSH daemon’s configuration file:

```Shell
sudo nano /etc/ssh/sshd_config
```

Inside the file, search for a directive called `PasswordAuthentication`. This may be commented out. Uncomment the line by removing any `#` at the beginning of the line, and set the value to `no`. This will disable your ability to log in through SSH using account passwords:

```Shell
PasswordAuthentication no
```

Apply the changes by running the following command:

```Shell
sudo systemctl restart ssh
```