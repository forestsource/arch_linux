# Run Playbook
```
#EDITOR=vim ansible-vault create secret.yml
ansible-playbook -i local site.yml -k --ask-become-pass
```
