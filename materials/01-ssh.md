# SSH


SSH (Secure shell) is a secure application layer protocol for connecting to a remote machine. SSH is the main way to connect remotely to Unix-like computers.


In order to connect to a machine via SSH, you can use two approaches:


1. Direct connection via password
2. Private key connection


To connect via password, simply enter the command `ssh <ip address of remote machine> ` then enter the password in the dialog box. The connection is established. When the connection is first established, the connecting host will receive a unique fingerprint for identification. Once connected, the user has access to the shell console on the remote machine inside his terminal.


*Note: Virtual machines may have the password connection disabled by default, so check the `/etc/ssh/sshd_config` file for the `PasswordAuthentication yes` line and `service sshd restart` if necessary to make changes to the config. The default password is usually vagrant.*


To connect by key, you must first create a private and public key pair `ssh-keygen -C <comment>` with the path to the file with the private key and a possible passphrase. The private key is a secret, and the public key (with the added extension .pub) must be sent to the machine to which it will connect with the command `ssh-copy-id -i <path to the file with the public key> <ip address of the remote host>`. After this, the system will ask for a connection password which must be entered once and from that moment the private key will be used in the default connection via the command `ssh <ip address of the remote machine>`.
