# Ansible

## Concepts
### Control Node or Mgmt Node
You can run commands and playbooks invoking `/usr/bin/ansible` or `/usr/bin/ansible-playbook`
> Linux only. Windows cannot be used as control node.
### Managed Nodes
Managed Nodes are controled through **ssh** or **WinRM** for Windows.
### Inventory
List of managed hosts also called host file
### Playbooks
Orded list of tasks, modules to be repeated used.
- **Tasks** are the units of actions
- **Modules** are the units of codes. You can invoke several modules with one task.
- **Idempotence** is the property of a operations tha can be applied several times without changing the result beyond the initial aplication
---

## Exercise
### Allowing ansible access to node (Pre-work)

Steps to grant access to ansible are:
    1. create ssh keys
    2. copy it into the node
    3. move/cp to the ansible user .ssh folder
    4. add ansible user to sudoers file

1. Generating SSH Key with `ssh-keygen`
    ```
    cardosm6@rsomw1611465:/mnt/c/Users/cardosm6/Documents/DevEnv/Ansible$ ssh-keygen
    Generating public/private rsa key pair.
    Enter file in which to save the key (/home/cardosm6/.ssh/id_rsa):
    Created directory '/home/cardosm6/.ssh'.
    Enter passphrase (empty for no passphrase):
    Enter same passphrase again:
    Your identification has been saved in /home/cardosm6/.ssh/id_rsa.
    Your public key has been saved in /home/cardosm6/.ssh/id_rsa.pub.
    The key fingerprint is:
    SHA256:KZyCQkvCp/aksDF0eOo4S8ISgTjlsYaVHjIau20eNEo cardosm6@rsomw1611465
    The key's randomart image is:
    +---[RSA 2048]----+
    |  +.             |
    |B=+o             |
    |O%+=             |
    |BE@. . . .       |
    |BX.o. + S        |
    |O*B  . .         |
    |*B o             |
    |+o.              |
    |.                |
    +----[SHA256]-----+
    cardosm6@rsomw1611465:/mnt/c/Users/cardosm6/Documents/DevEnv/Ansible$
    ``` 
2. Copying .pub key into destination host with `ssh-copy-id -f -i id_rsa.pub [use]@[destination]`
    ```
    cardosm6@rsomw1611465:/mnt/c/Users/cardosm6/Documents/DevEnv/Ansible$ cd /home/cardosm6/.ssh/id_rsa
    -bash: cd: /home/cardosm6/.ssh/id_rsa: Not a directory
    cardosm6@rsomw1611465:/mnt/c/Users/cardosm6/Documents/DevEnv/Ansible$ cd /home/cardosm6/.ssh/
    cardosm6@rsomw1611465:~/.ssh$ ls -l
    total 4
    -rw------- 1 cardosm6 cardosm6 1766 Dec 11 10:13 id_rsa
    -rw-r--r-- 1 cardosm6 cardosm6  403 Dec 11 10:13 id_rsa.pub
    cardosm6@rsomw1611465:~/.ssh$ ssh-copy-id -f -i id_rsa.pub cardosm6@[destination]
    ```
    Your connection will be placed without asking for credentials.

3. Once you are logged in, change the user to root using `su root`, create the folder `mkdir /home/users/ansible/.ssh`
5. Copy the id_rsa from the user you copied into ansible .ssh folder using `cp /home/users/cardosm6/.ssh/authorized_keys /home/users/ansible/.ssh/`, change owner `chown ansible:users /home/ansible/.ssh/authorized_keys`
7. Add ansible user to suduers: `echo "ansible ALL=NOPASSWD:ALL" >> /etc/sudoers`. Confirm with `cat /etc/sudoers`

Now ansible user have access to the host. Repeat the process for all the nodes you want to manage.

### Installing Ansible

Run the command `pip install ansible`.






