Best Practices for setting up Ansible and SSH keys.
Source: https://www.youtube.com/watch?v=-Q4T9wLsvOQ&list=PLT98CRl2KxKEUHie1m24-wkyHpEsa4Y70&index=2


1. Install ssh-server: 
sudo apt install -y openssh-server


2. Generate ssh key for servers:
ssh-keygen -t ed25519 -C "uz default"
("-t" stands for type of SSH key. "ed25519" key type format is more secure format which shortens the ssh.pub key. "-C" stands for comment for the SSH key. Give Name: "uz")


3. Accept the default location of the SSH key and make sure to create passphrase to make the SSH key much more secure to connect master to servers.
cat .ssh/id_ed25519.pub (to see the public key)


4. Copy the SSH pub key from Master to the servers.
ssh-copy-id -i ~/.ssh/id_ed25519.pub "server ip-address" 
("-i" stands for input file, then location of the ssh pulic key in Master and then ip-address of the target server)


5. Now create SSH key dedicated only for Ansible.
ssh-keygen -y ed25519 -C "ansible"
("-t" stands for type of SSH key. "ed25519" key type format is more secure format which shortens the ssh.pub key. "-C" stands for comment for the SSH key. 
Give Name: "ansible". Then do not choose default location for ssh key (it will override previous ssh key) instead create new one by typing /home/uz/.ssh/ansible)
MAKE SURE THAT YOU DO NOT CREATE PASSPHRASE FOR THE ANSIBLE SSH KEY so that Ansible can automatically connect to servers without passphrase.


6. Copy the SSH pub key from Master to the servers.
ssh-copy-id -i ~/.ssh/ainsible.pub "server ip-address" 
("-i" stands for input file, then location of the ssh pulic key in Master and then ip-address of the target server)


7. You can confirm the copied key going to your server's CLI and type:
cat .ssh/authorized_keys


8. You have to choose the right ssh key to connect to the servers. Now we have two keys: original master to server ssh key and ansible ssh key. To do so type:
For ansible: ssh -i ~/.ssh/ainsible "server-ip-address" (as we did not create passphrase for ansible it connects automatically)
For server:  ssh "server-ip-address" (then enter passphrase)


9. If I do not want to enter passphrase to connect from Master to server everytime I can edit my .bashrc file and for to alias section and add:
nano .bashrc
###ssh agent alias
alias ssha='eval $(ssh-agent) && ssh-add'
(Save and Exit then type ssha on the terminal to connect)
