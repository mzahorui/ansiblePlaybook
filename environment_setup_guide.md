## Setting up ssh connection
### On server side:
- Install ssh-server:
  ```
  sudo apt install openssh-server -y
  ```
- Start and enable SSH:  
  ```
  sudo systemctl start ssh
  sudo systemctl enable ssh
  ```
- Check if 
   ```
   /etc/ssh/sshd_config
   ```
   has 
   ```
   AllowAgentForwarding yes
   ```
- Before enabling firewall, allow only local network connection:  
  ```
  sudo ufw allow from <home network mask> to any port 22
  ```
- Enable firewall:  
  ``` 
  sudo ufw enable
  ```

### On client side:
- Load the github key to ssh agent:  
  ``` 
  ssh-add ~/.ssh/<privatekey>
  ```
- Configure .ssh/config:
  ```
  Host <alias for the server>  
      HostName <IP of the server>  
      User <server's user name>  
      ForwardAgent yes
   ```


## Ansible commands
ansible-playbook -i inventory.ini setup.yaml -k -K


ansible-galaxy collection install community.general
ansible-playbook -i inventory.ini setup.yaml -k -K

# Forward Host Port 2222 -> Guest Port 22
virsh --connect qemu:///session qemu-monitor-command debian-server --hmp "hostfwd_add tcp::2222-:22"

virsh --connect qemu:///session destroy debian-server
virsh --connect qemu:///session undefine debian-server
