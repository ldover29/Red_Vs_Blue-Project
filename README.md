# Red_Vs_Blue-Project
The assessment, analysis, and hardening of a vulnerable system from the red and blue side.

# Scenario

Assuming the role of both pentester (The Red Team) and SOC analyst (The Blue team), The Red Team will attack a vulnerable virtual machine, ultimately gaining root access to the machine. As Blue Team, Kibana logs will be reviewed taken during the Day 1 engagement. The logs will be used to extract hard data and visualizations. Log data will be interpreted for mitigation measures for each exploit that was successfully performed.

# Network Topology

|  IP Address   |       Machine       |
|:-------------:|:-------------------:|
|  192.168.1.1  |       Hyper-V       |
|  192.168.1.90 |         Kali        |
| 192.168.1.100 | Elastic Stack (ELK) |
| 192.168.1.105 |       Capstone      |

![name-of-you-image](https://github.com/ldover29/Red_Vs_Blue_Project/blob/d376cac01957cbf835b0715394d88edb26bec282/Images/Project%202%20-%20Network%20Diagram.jpg)

# Red Team - Penetration Test


