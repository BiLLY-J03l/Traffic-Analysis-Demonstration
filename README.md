# Traffic-Analysis-Demonstration
Analyzing and Capturing Traffic with Wireshark and Tcpdump


## Update the instance and install vsftpd
    sudo su
    apt update; apt full-upgrade -y ; apt autoremove -y
    apt install vsftpd

## Configure vsftpd.conf file

  - The config file will be included in the repo.

## Add a user to be logged in with into the ftp server

    adduser ftp_user_billy
    mkdir /home/ftp_user_billy/ftp
    chown nobody:nogroup /home/ftp_user_billy/ftp
    
    #Removes write permission for all users (a stands for "all" users, and -w means "remove write permission")
    chmod a-w /home/ftp_user_billy/ftp
    
    
    mkdir /home/ftp_user_billy/ftp/upload
    echo "ftp_user_billy" | tee -a /etc/vsftpd.userlist
    
    #add two lines involving the vsftpd.userlist in the vsftpd.conf (included in the repo)
    
    systemctl restart vsftpd
---------------------------------------------------------------------------

## start the capture with Tcpdump

        tcpdump -s 0 -i enp0s3 -w demo.pcap

 - log in into the ftp server with ftp cmdline or filezilla, I used filezilla

 ![image](https://github.com/user-attachments/assets/8269c46d-9da5-4b99-9fd5-0bf8ca9d8e69)

---------------------------------------------------------------------------

## open the demo.pcap capture file with wireshark


![image](https://github.com/user-attachments/assets/b127e4c9-9d26-4423-9a9d-473779e1cb82)

- That's a lot of data but we're interested in the ftp bit only.

    - ![image](https://github.com/user-attachments/assets/49f7dd8c-b7d1-4c96-b3ba-98d587b16bf5)

- Here we got the user

    - ![image](https://github.com/user-attachments/assets/c19c9286-20e4-4df5-85fd-c00e1f54cfe2)

- and the password

    - ![image](https://github.com/user-attachments/assets/1ac4487e-b07f-45ff-83e5-11be603c0d46)

---------------------------------------------------------------------------

## Mitigation

- enable TLS/SSL to encrypt the data in transit

        openssl req -x509 -nodes -days 9999 -newkey rsa:2048 -keyout /etc/ssl/private/vsftpd.pem -out /etc/ssl/private/vsftpd.pem

- add furthur config to the vsftpd.conf

    - ![image](https://github.com/user-attachments/assets/49489744-1f86-49f0-8904-68bdd4965afa)
    - ![image](https://github.com/user-attachments/assets/ce17eb22-5322-43a0-97f4-58b101d99728)

- here we get that there's an ssl cert in use by the server in the filezilla client

    - ![image](https://github.com/user-attachments/assets/e233aa7e-a976-4e0e-8b34-ba0b1af62f66)
    - ![image](https://github.com/user-attachments/assets/e32decca-69ba-49de-a236-fc0b145620a2)

- starting the traffic analysis in wireshark, we find all the traffic as "Application Data" which means the traffic is encrypted with SSL/TLS

    - ![image](https://github.com/user-attachments/assets/9fcde26b-4163-4e7a-83af-4038960cbe9a)
    - ![image](https://github.com/user-attachments/assets/0f24cc1d-381a-46f2-96f4-7f5e54e19e28)



