# Traffic-Analysis-Demonstration
Analyzing and Capturing Traffic with Wireshark and Tcpdump


## Update the instance and install vsftpd
    sudo su
    apt update; apt full-upgrade -y ; apt autoremove -y
    apt install vsftpd

## Configure vsftpd.conf file

  -The config file will be included in the repo.

## Add a user to be logged with in the ftp server

    adduser ftp_user_billy
    mkdir /home/ftp_user_billy/ftp
    chown nobody:nogroup
    
    #Removes write permission for all users (a stands for "all" users, and -w means "remove write permission")
    chmod a-w /home/ftp_user_billy/ftp
    
    
    mkdir /home/ftp_user_billy/ftp/upload
    echo "ftp_user_billy" | tee -a /etc/vsftpd.userlist
    
    #add two lines involving the vsftpd.userlist in the vsftpd.conf (included in the repo)
    
    systemctl restart vsftpd
