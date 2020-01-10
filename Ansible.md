# Run Playbook
```
echo '<SecurePass>' > ~/.ansible_vault_pass
EDITOR=vim ansible-vault create secret.yml
## ansible_sudo_pass: <SecurePass>
## ansible_ssh_pass: <SecurePass>

ansible-playbook -i local local.yml --extra-vars='@secret.yml'
```
