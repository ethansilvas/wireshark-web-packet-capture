# Wireshark Web Packet Capture

Setup Wireshark on a Linux machine and complete packet captures on an ethernet port for HTTP/HTTPS network traffic

## Task 1 - Setup Wireshark onto Ubuntu Linux System

The system used in this project will require Wireshark to be installed and set up for users belonging to the **wireshark** group. 

This system should be set up so that we do not need elevated permissions to run Wireshark, so I specify that non-superusers should be able to capture packets when installing:

![](Images/Pasted%20image%2020230728150736.png)

Now I ensure that the latest stable version of Wireshark is installed with `sudo add-apt-repository ppa:wireshark-dev/stable`.

![](Images/Pasted%20image%2020230728150931.png)

Next, I need to ensure that the current user **rhyme** is added to the **wireshark** group. To do this I modify the group to add the current user with `sudo usermod -aG wireshark $USER`.

This command can be broken down like so: 
	- `sudo usermod` = modify the current user with elevated permissions
	- `-aG` = append a group
	- `wireshark` = the group to be added to the user
	- `$USER` = modify the current user (rhyme)

The current user, **rhyme**, now has packet capture capabilities with Wireshark. 

## Task 2 - Start Packet Captures on an Ethernet Port

For this system, there are 5 wired network interfaces that I can view and the ethernet port is listed as **ens5**:

![](Images/Pasted%20image%2020230728152328.png)

I initiate a quick packet capture on **ens5** to verify functionality and save the results to file:  

![](Images/Pasted%20image%2020230728153230.png)

## Task 3 - Use Display Filters to Detect HTTPS Packets



