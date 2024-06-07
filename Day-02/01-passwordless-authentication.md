# How to setup Passwordless Authentication

## EC2 Instances

### Using Public Key

```
ssh-copy-id -f "-o IdentityFile <PATH TO PEM FILE>" ubuntu@<INSTANCE-PUBLIC-IP>
```

- ssh-copy-id: This is the command used to copy your public key to a remote machine.
- -f: This flag forces the copying of keys, which can be useful if you have keys already set up and want to overwrite them.
- "-o IdentityFile <PATH TO PEM FILE>": This option specifies the identity file (private key) to use for the connection. The -o flag passes this option to the underlying ssh command.
- ubuntu@<INSTANCE-IP>: This is the username (ubuntu) and the IP address of the remote server you want to access.

### Using Password 

- Go to the file `/etc/ssh/sshd_config.d/60-cloudimg-settings.conf`
- Update `PasswordAuthentication yes`
- Restart SSH -> `sudo systemctl restart ssh`




how to create the sshd.
The error "zsh: no matches found: /Users/akhilreddyannapureddy/.ssh/id_*" suggests that there are no files matching the pattern `/Users/akhilreddyannapureddy/.ssh/id_*` in your `.ssh` directory. This likely means you do not have any SSH keys generated yet.

Let's generate a new SSH key pair and then copy it to your remote server:

### 1. Generate a New SSH Key Pair
Run the following command to generate a new SSH key pair. You can use RSA or Ed25519. Here is an example using RSA:
```sh
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```
Replace `"your_email@example.com"` with your email address. This command will prompt you for a file location to save the key (default is `~/.ssh/id_rsa`) and an optional passphrase.

### 2. Add the SSH Key to the SSH Agent
After generating the key, add it to the SSH agent:
```sh
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_rsa
```
If you used a different name or location for your key, adjust the path accordingly.

### 3. Copy the SSH Key to the Remote Server
Use `ssh-copy-id` to copy your SSH key to the remote server:
```sh
ssh-copy-id user@remote_host
```
Replace `user` with your remote username and `remote_host` with the remote server's address.

### 4. Verify the SSH Connection
Finally, test the SSH connection to ensure key-based authentication works:
```sh
ssh user@remote_host
```

### Additional Notes
- Ensure the `.ssh` directory and the key files have the correct permissions. For example:
  ```sh
  chmod 700 ~/.ssh
  chmod 600 ~/.ssh/id_rsa
  chmod 644 ~/.ssh/id_rsa.pub
  ```
- If you encounter any issues, make sure your SSH configuration in `~/.ssh/config` (if it exists) is correct and that the remote server is configured to accept key-based authentication.

By following these steps, you should be able to generate a new SSH key pair, add it to the SSH agent, and copy it to your remote server, resolving the initial issue.

