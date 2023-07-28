# Wireshark Web Packet Capture

Setup Wireshark on a Linux machine and complete packet captures on an ethernet port for HTTP/HTTPS network traffic

## Part 1 - Setup Wireshark and Packet Capture HTTPS Traffic

### Setup Wireshark onto Ubuntu Linux System

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

### Start Packet Captures on an Ethernet Port

For this system, there are 5 wired network interfaces that I can view and the ethernet port is listed as **ens5**:

![](Images/Pasted%20image%2020230728152328.png)

I initiate a quick packet capture on **ens5** to verify functionality and save the results to file:  

![](Images/Pasted%20image%2020230728153230.png)

### Use Display Filters to Detect HTTPS Packets

First I will initiate a packet capture and generate HTTPS traffic by going to duckduckgo.com, then I will save the packet capture to filter for HTTPS traffic:

![](Images/Pasted%20image%2020230728154829.png)

Using the display filter, I filter for all HTTPS traffic by specifying `tcp.port == 443`.

![](Images/Pasted%20image%2020230728155028.png)

Looking through the results I find a TLSv1.3 protocol (the TLS handshake that HTTPS uses) with info of "Client Hello". This is the first part of the traffic I created by visiting duckduckgo.com and I can verify this with the destination IP address of **52.149.246.39** which is duckduckgo.com's IP address. 

![](Images/Pasted%20image%2020230728155251.png)

## Part 2 - Packet Capture HTTP Traffic

Now that I have verified that I can capture HTTPS traffic successfully, I will packet capture HTTP traffic as well. 

First I start a new packet capture, then navigate to `http://cygwin.com` to generate HTTP traffic. 

![](Images/Pasted%20image%2020230728161549.png)

Then to search for the traffic I generated I apply the display filter `tcp.port == 80 && http contains "cygwin"`:

![](Images/Pasted%20image%2020230728161705.png)

I can verify that this is the correct traffic by looking at the first GET packet's HTTP data: 

![](Images/Pasted%20image%2020230728161828.png)

## Part 3 - Detect Web Page IP Address Using Display Filters

In this section I want to see the IP addresses of HTTPS traffic generated during a capture. 

First, I will initiate a new packet capture and generate traffic by going to google.com. 

Next, I will stop the packet capture and use the display filter to search for HTTPS traffic and view all of the initial TLS handshakes with `tcp.port == 443 && tls.handshake.type == 1`.

![](Images/Pasted%20image%2020230728163341.png)

In this example the destination IP address for google.com is 142.251.16.105. With this info I can use different display filters to view different packets: 
	- `ip.addr == 142.251.16.105` to view all packets related to the IP address
	- `ip.src == 142.251.16.105` to view all packets from the IP address
	- `ip.dst == 142.251.16.105` to view all packets going to the IP address

![](Images/Pasted%20image%2020230728163943.png)

