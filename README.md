# PiStreamNIA

- [PiStreamNIA](#pistreamnia)
  - [Worker Setup](#worker-setup)
  - [Manager Setup](#manager-setup)
    - [Setting up SSH communications](#setting-up-ssh-communications)
  - [Deploy Stream Return Configurations](#deploy-stream-return-configurations)
    - [Install and configure repository](#install-and-configure-repository)
    - [Running the provisioning scripts](#running-the-provisioning-scripts)
  - [General Information](#general-information)
    - [Secrets!!](#secrets)
    - [Applications Installed](#applications-installed)
      - [Managers](#managers)
      - [Workers](#workers)
    - [Application Structure](#application-structure)

## Worker Setup

On an existing computer download the [Raspberry Pi Imager](https://www.raspberrypi.com/software/), insert a SD Card, and launch the software.

Select the appropriate RPi hardware and then `Raspberry Pi OS Full`. Then select the SD Card and select `Create`. When you are asked to apply settings select `Edit Settings`. Set the username and password, make sure wifi is off, and select to install a SSH server. Finally set and note the `hostname`.

Boot and connect:
Connect the RPi to the streaming network and boot it up.

Open a terminal on another computer on the same network log in:

```sh
ssh {username}@{hostname}.local
```

Now set a staic IP address:

```sh
sudo nmlci
```

Reboot the RPi

```sh
sudo reboot
```

## Manager Setup

These instructions assume that the management PC is a Mac.

Install and configure the computer with the appropriate IP address.
Note: Pay special attention to the username (use the actual username, not the “Full Name”) and the password. These must match what is in the secrets.yml file (they can be changed there as needed).

Generate an SSH key:
Open a terminal, create an SSH key, and follow the prompts (no password is required).

```sh
ssh-keygen -t ed25519 -C "gmail@gmail.com"
```

Install Xcode Command Line Tools:

```sh
xcode-select --install
```

Install Homebrew:

```sh
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

Upgrade Pip:

```sh
sudo pip3 install --upgrade pip
```

Create a code directory and enter it:

```sh
mkdir ~/code &&
cd ~/code
```

### Setting up SSH communications

From the management terminal copy SSH key to the workers.

Copy your SSH key to the RPi from the management PC:

```sh
ssh-copy-id admin@{IP Address}
```

This needs to be repeated for each worker computer.

## Deploy Stream Return Configurations

### Install and configure repository

Work on the management computer.

Open a terminal and navigate to the code directory

```sh
cd ~/code
```

Clone the repository and enter the directory:

```sh
git clone https://github.com/PiCommCapp/PiStreamNIA.git &&
cd PiStreamNIA
```

Create and edit the password file:

```sh
touch passwd
sudo nano passwd
```

Enter the super secret password in the file, then close it by pressing `ctrl+x` and selecting `y` to save.

Create and activate a Python virtual environment:

```sh
python3 -m venv .venv &&
source .venv/bin/activate
```

Install Python requirements and test Ansible installation:

```sh
pip3 install -r requirements.txt &&
ansible --version
```

Install Ansible roles and collections:

```sh
ansible-galaxy install -r requirements.yml &&
ansible-galaxy list
```

### Running the provisioning scripts

Execute the provisioning scripts

For managers:

```sh
ansible-playbook setupmanagers.yml
```

For workers:

```sh
ansible-playbook setupworkers.yml
```

## General Information

### Secrets!!

The file `secrets.yml` contains all sensative information and is encrypted. Once the password file `passwd` is created the scripts will see the information in the files transparently.

To decrypt the file to edit, from a terminal on the management PC navagate to the code directory

```sh
cd ~/code/PiStreamNIA
```

Decrypt the file with:

```sh
ansible-vault decrypt secrets.yml
```

To encrypt:

```sh
ansible-vault encrypt secrets.yml
```

Care must be taken not so share the file when unencrypted!

### Applications Installed

Lists do not include dependencies that are also installed

#### Managers

- Python              # Scripting language
- Docker              # Container process backend
- Google Chrome       # Web thingy
- VLC                 # Video player
- anydesk             # Remote access
- Github Deskop       # Repository access
- VS Code             # Programming IDE
- RPi Imager          # SD Card imager
- TigerVNC Viewer     # Remote access to workers
- Wireshark           # Network analyser  
- Angry IP Scanner    # Network scanner
- Tailscale           # Mesh VPN service
- FFmpeg              # Video transcoder

#### Workers

- Python
- FFmpeg
- VLC

### Application Structure

```sh
|- .venv/                                  # Python VENV folder
|- .github
|      |- workflows
|            |- check_ansible_secrets.yml  # Attempt to prevent unencrypted secrets on the repo
|- .vscode                                 # VSCode settings folder
|      |- extensions.json                  # Recommended Extensions
|      |- settings.json                    # Ansible provisioning settings
|      |- tasks.json                       # VSCode Tasks
|- files                                   # Files for placement
|      |- BGCard.png                       # Desktop Image
|      |- NIAAssemblyVimeo.xml             # BMD WebPresentor XML file (currently not working!!!)
|      |- OffAirCard.png                   # Off Air Image
|- group_vars                              # Variables to apply to groups
|      |- linux.yml                        # Variables for all Linux computers
|      |- macos.yml                        # Variables for all Mac computers
|      |- managers.yml                     # Variables for management computers
|      |- workers.yml                      # Variables for all worker computers
|- host_vars                               # Variables to apply to specific computers
|      |- streamreturn1.yml
|      |- streamreturn2.yml
|      |- streamreturn3.yml
|      |- streamreturn4.yml
|      |- streammanager1.yml
|- tasks                                   # Task playbooks
|      |- desktop.yml                      # Set Desktop Image on workers
|      |- homebrew.yml                     # Setup homebrew and install mac applications
|      |- linux.yml                        # General Linux provisioning tasks
|      |- offair.yml                       # Place Off Air image card
|      |- rpi.yml                          # Raspberry Pi specific configurations
|      |- streamreturn.yml                 # Places the python script, maybe the only step that matters
|      |- sysmenu.yml                      # Place helper script on computers
|      |- sshconfig.yml                    # Create SSH config file from template
|- templates                               # Jinja2 template files
|      |- desktop-items-0.conf.j2          # Worker desktop environment configuration
|      |- service.sh.j2                    # Linux support template
|      |- sshconfig.j2                     # SSH config template
|      |- streamreturn.desktop.j2          # Autostart script
|      |- streamreturn.py.j2               # Streaming script
|- .gitignore
|- ansible.cfg                             # Ansible config file
|- hosts                                   # Inventory file
|- passwd                                  # Vault password file (not synced)
|- README.md                               # You are here
|- requirements.txt                        # Python dependencies
|- requirements.yml                        # Ansible roles and collections
|- secrets.yml                             # Encrypted secrets file
|- setupmanagers.yml                       # Playbook to set up management computers
|- setupworkers.yml                        # Playbook to set up workers
