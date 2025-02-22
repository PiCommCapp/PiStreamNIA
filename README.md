# PiStreamNIA
 
## Preparation

## Structure

```bash
|- .venv/                               # Python VENV folder
|     |- bin
|     |     |- activate
|     |     |- pip3
|     |     |- python3
|- .vscode                              # VSCode settings folder
|     |- settings.json
|- ansible                              # Ansible provisioning Folder
|     |- files                          # Whole files for placement
|     |     |- NIAAssemblyVimeo.xml      # BMD WebPresentor XML file (currently not working!!!)
|     |- group_vars                     # Variables to apply to groups
|     |     |- linux.yml                # Variables for all linux computers
|     |     |- macos.yml                # Variables for all Mac computers
|     |     |- managers.yml             # Variables for Management computers
|     |     |- workers.yml              # Variables for all Worker computers
|     |- host_vars                      # Variables to apply to specific computers
|     |     |- streamreturn1.yml
|     |     |- streamreturn2.yml
|     |     |- streamreturn3.yml
|     |     |- streamreturn4.yml
|     |     |- streammanager1.yml
|     |- tasks                          # Task playbooks
|     |     |- linux.yml                # General linux provisioning tasks
|     |     |- sysmenu.yml              # Place helper script on computers
