I am having problems with ansible and my venv in VSCODE

I am getting the following error

```bash
Command failed: ${workspaceFolder}/.venv/bin/ansible-playbook /Users/pistream4/code/PiStreamNIA/setupworkers.yml --syntax-check
/bin/sh: /.venv/bin/ansible-playbook: No such file or directory
```

My settings.json

```json
{
  "files.associations": {
    "*.yml": "ansible",
    "*.yaml": "ansible",
    "*.j2": "ansible-jinja"
  },

  "yaml.schemas": {
    "https://raw.githubusercontent.com/ansible/schemas/main/f/ansible.json": [
      "playbooks/*.yml",
      "tasks/*.yml"
    ]
  },

  "ansible.validation.lint": "ansible-lint",
  "ansible.validation.enabled": true,
  "ansible.useFullyQualifiedCollectionNames": true,
  "ansible.hover.documentation": true,
  "ansible.completion.enabled": true,
  "ansible.ansible.path": "${workspaceFolder}/.venv/bin/ansible",

  "[yaml]": {
    "editor.defaultFormatter": "redhat.vscode-yaml",
    "editor.formatOnSave": true,
    "editor.formatOnPaste": true,
    "editor.formatOnType": true,
    "editor.tabSize": 2,
    "editor.insertSpaces": true
  },

  "[jinja]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },

  "[python]": {
    "editor.tabSize": 2,
    "editor.insertSpaces": true
  },

  "editor.tabSize": 2,
  "editor.insertSpaces": true,
  "editor.detectIndentation": false,
  "editor.formatOnSave": true,
  "editor.formatOnPaste": true,
  "editor.formatOnType": true,

  "terminal.integrated.defaultProfile.linux": "bash",
  "terminal.integrated.shellArgs.linux": [
    "--rcfile",
    "${workspaceFolder}/.venv/bin/activate"
  ],

  "python.defaultInterpreterPath": "${workspaceFolder}/.venv/bin/python3",
  "python.analysis.extraPaths": [
    "${workspaceFolder}/.venv/lib/python3.13/site-packages"
  ],
  "python.venvPath": "${workspaceFolder}/.venv",
  "python.venvFolders": [".venv", "venv"],
  "python.terminal.activateEnvironment": true,
  "python.formatting.provider": "black",
  "python.linting.enabled": true,
  "python.linting.pylintEnabled": true,
  "python.linting.flake8Enabled": false,

  "ansible.python.interpreterPath": "${workspaceFolder}/.venv/bin/python",
  "ansible.executionEnvironment.enabled": false,

  "git.enableSmartCommit": true,
  "git.autofetch": true,
  "editor.codeLens": true
}
```

and my ansible.cfg

'''ini
[defaults]
inventory = ~/code/PiStreamNIA/hosts
stdout_callback=debug
stderr_callback=debug
# host_key_checking = False
remote_user = root
vault_password_file = ~/code/PiStreamNIA/passwd
ANSIBLE_GALAXY_TOKEN_PATH = ag_token

[ssh_connection]
control_path = ~/.ssh/ansible-%%r@%%h:%%p
ssh_args = -C -o ControlMaster=auto -o ControlPersist=60s
pipelining = False

[paramiko_connection]
record_host_keys = False
```

Project structure

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
