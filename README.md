# InstaTools

An Ansible playbook for automated installation and configuration of various security testing tools. This playbook sets up a complete security testing environment with various tools organized into specific roles.

## Structure
```
.
├── ansible.cfg
├── inventory.yml
├── main.yml
└── roles/
    ├── environment_prep/
    ├── go_tools/
    ├── pipx_tools/
    ├── miscellaneous/
    ├── metasploit/
    ├── seclists/
    └── cleanup/
```

## Roles

### environment_prep
#### IF NOT RUNNING FULL PLAYBOOK, THIS ROLE MUST BE RUN PRIOR TO RUNNING INDIVIDUAL ROLES.
Sets up the basic environment and dependencies:
- System packages and dependencies
- Go installation and configuration
- Python with pyenv installation
- TMux with custom configurations
- Tmuxinator setup
- Pipx installation
- Tag: `environment_prep`

### go_tools
Installs Go-based security tools:
- ProjectDiscovery tools (nuclei, subfinder, httpx, naabu, etc.)
- Subjack for subdomain takeover scanning
- FFuf for web fuzzing
- Gowitness for web screenshots
- TruffleHog for secret scanning
- Aquatone for visual web inspection
- Tag: `go_tools`

### pipx_tools
Installs Python-based tools using pipx:
- BBOT for bug bounty recon
- WAFW00F for WAF detection
- NetExec (formerly CrackMapExec)
- Tag: `pipx_tools`

### miscellaneous
Various security tools:
- Feroxbuster for web content discovery
- MassDNS for DNS resolving
- Hostess-Pie for host discovery
- SQLMap for SQL injection testing
- Fast Google Dorks Scan (FGDS)
- Trevor tools (trevorproxy, trevorspray)
- Tag: `miscellaneous`

### metasploit
Installs and configures Metasploit Framework:
- Latest version installation
- Environment configuration
- PATH setup
- Tag: `metasploit`

### seclists
Installs SecLists:
- Collection of multiple types of lists for security testing
- Wordlists for content discovery
- Payload lists
- Pattern matching lists
- Tag: `seclists`

### cleanup
Maintenance role for cleaning up after installation:
- Removes temporary downloaded files
- Cleans up build directories
- Verifies successful installations before cleanup
- Tag: `cleanup`

## Usage

### Install All Tools
```bash
ansible-playbook main.yml -e "target_ip=10.x.x.x ssh_user=localuser ssh_key_path=/home/acme/.ssh/id_ed25519" --ask-become-pass
```

### Install Using Tags
You can use tags to run specific roles. This is often preferred over using selected_roles.

#### Install Single Role
```bash
ansible-playbook main.yml -e "target_ip=10.x.x.x ssh_user=localuser ssh_key_path=/home/acme/.ssh/id_ed25519" --tags "environment_prep" --ask-become-pass
```

#### Install Multiple Roles
```bash
ansible-playbook main.yml -e "target_ip=10.x.x.x ssh_user=localuser ssh_key_path=/home/acme/.ssh/id_ed25519" --tags "environment_prep,go_tools" --ask-become-pass
```

#### List Available Tags
```bash
ansible-playbook main.yml --list-tags
```

#### Skip Specific Tags
```bash
ansible-playbook main.yml -e "target_ip=10.x.x.x ssh_user=localuser ssh_key_path=/home/acme/.ssh/id_ed25519" --skip-tags "cleanup" --ask-become-pass
```
## Requirements
- Ansible 2.9+
- SSH access to target machine
- Sudo privileges on target machine

## Note
- The cleanup role should typically run last in the sequence
- Always ensure environment_prep is run before installing tools
- Tags can be combined in any order, but roles will execute in the order defined in main.yml
