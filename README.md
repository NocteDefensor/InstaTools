# InstaTools
Ansible Script to install my favorite Tools.

## Example Command
```bash
ansible-playbook site.yml -e "target_ip=10.x.x.x ssh_user=localuser ssh_key_path=/home/acme/.ssh/id_ed25519" --ask-become-pass
```
- In the above example "--ask-become-pass will prompt you to enter the password if required to sudo to root.
- It will prompt you to enter password for ssh key if its password-protected.
