# PiStreamNIA

- [PiStreamNIA](#pistreamnia)
  - [Preparation](#preparation)
    - [Manager Setup](#manager-setup)
    - [Worker Setup](#worker-setup)
  - [Deploy Stream Return configurations](#deploy-stream-return-configurations)
  - [Structure](#structure)

## Preparation

### Manager Setup

These instructions assume the management PC is a Mac.

Install and setup the computer and configure with the appropriate IP address.

Open a terminal and create a SSH key and follow the prompts, no password is required.

```sh
ssh-keygen -t ed25519 -C "gmail@gmail.com"
```

Install Mac XCode CLI

```sh
xcode-select --install
```

Install [Homebrew](https://brew.sh/)

```sh
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

Upgrade Pip

```sh
sudo pip3 install --upgrade pip
```

Create a code directory and enter it

```sh
mkdir ~/code && \
cd ~/code
```


### Worker Setup

Setup the Raspberry pi with RaspberryPiOS with full desktop and configure the correct IP address and a user of `admin`.

Connect the RPi to the streaming network and boot.

Copy your SSH Key to the new RPi from the management PC

```sh
ssh-copy-id admin@{IP Address}
```

Connect to the RPi and update onboard software

```sh
ssh admin@{IP Address}
sudo apt update && sudo apt upgrade -y
```

Setup basic configurations on the RPi, making sure to set login to `desktop - auto login` and to expand the file system.

```sh
sudo raspi-config
```

Exit the RPi

```sh
exit
```

## Deploy Stream Return configurations

Clone the repository and enter the directory

```sh
git clone https://github.com/PiCommCapp/PiStreamNIA.git
cd PiStreamNIA
```

Create and enter Password file

```sh
touch passwd
sudo nano passwd
```

Create a Python VENV and activate it

```sh
python3 -m venv .venv
source .venv/bin/activate
```

Install python requirements and test Ansible for installation

```sh
pip3 install -r requirements.txt
ansible --version
```

Install Ansible roles and collections

```sh
ansible-galaxy install -r requirements.yml
ansible-galaxy list
```

## Structure

```sh
|- .venv/                         # Python VENV folder
|     |- bin
|     |     |- activate
|     |     |- pip3
|     |     |- python3
|- .vscode                        # VSCode settings folder
|     |- extensions.json          # Reccomended Extensions
|     |- settings.json            # Ansible provisioning Folder
|     |- tasks.json               # VSCode Tasks
|- files                          # Whole files for placement
|     |- BGCard.png               # Desktop Image
|     |- NIAAssemblyVimeo.xml     # BMD WebPresentor XML file (currently not working!!!)
|     |- OffAirCard.png           # Off Air Image
|- group_vars                     # Variables to apply to groups
|     |- linux.yml                # Variables for all linux computers
|     |- macos.yml                # Variables for all Mac computers
|     |- managers.yml             # Variables for Management computers
|     |- workers.yml              # Variables for all Worker computers
|- host_vars                      # Variables to apply to specific computers
|     |- streamreturn1.yml
|     |- streamreturn2.yml
|     |- streamreturn3.yml
|     |- streamreturn4.yml
|     |- streammanager1.yml
|- tasks                          # Task playbooks
|     |- desktop.yml              # Set Desktop Image on Workers
|     |- linux.yml                # General linux provisioning tasks
|     |- offair.yml               # Place Off Air image card
|     |- sysmenu.yml              # Place helper script on computers
|     |- sshconfig.yml            # Create SSH Config file from template
|     |- sysmenu.yml              # Create helpful linux menu from template
|- templates                      # Jinja2 template file
|     |- service.sh.j2            # Linux support template
|     |- sshconfig.j2             # SSH config template
|     |- streamreturn.py.j2       # Streaming script
|- .gitignore
|- ansible.cfg                    # Ansible config file
|- hosts                          # Inventory File
|- passwd                         # Vault password file, not sync'ed
|- README.md                      # You are here
|- requirements.txt               # Python dependencies
|- requirements.yml               # Ansible roles and collections
|- secrets.yml                    # Encrypted secrets file
|- setupmanagers.yml              # Playbook to setup management computers
|- setupworkers.yml               # Playbook to setup workers
```
