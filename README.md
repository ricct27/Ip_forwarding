https://askubuntu.com/questions/359856/share-wireless-internet-connection-through-ethernet
https://askubuntu.com/questions/1005994/unable-to-share-wifi-through-ethernet

# Enable IP forwarding. Set the value 1
echo "1" > /proc/sys/net/ipv4/ip_forward


sudo iptables -L

# After that add a rule telling to forward the traffic
sudo iptables -A FORWARD -i eth0 -o wlp59s0 -j ACCEPT
sudo iptables -A FORWARD -i wlp59s0 -o eth0 -m state --state ESTABLISHED,RELATED -j ACCEPT

# Because you router do not known for your lan network we must do masquarade
sudo iptables -t nat -A POSTROUTING -o wlp59s0 -j MASQUERADE

# List iptables content
sudo iptables -L

# Show the rules
sudo iptables -S

# Make them persistent over reboot
apt-get install iptables-persistent
iptables-save > /etc/iptables/rules.v4
