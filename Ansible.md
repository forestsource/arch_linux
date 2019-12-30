# Run Playbook
```
EDITOR=vim ansible-vault create secret.yml
ansible-playbook -i local site.yml -u ansible -k --ask-vault-pass
```
