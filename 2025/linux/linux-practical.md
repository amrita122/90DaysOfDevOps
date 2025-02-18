Task:
Create a user devops_user and add them to a group devops_team.
Set a password and grant sudo access.
Restrict SSH login for certain users in /etc/ssh/sshd_config.
sudo useradd -m devops_user 
sudo passwd devops_user
sudo groupadd devops_team
sudo gpasswd -M devops_user devops_team
cat /etc/group
