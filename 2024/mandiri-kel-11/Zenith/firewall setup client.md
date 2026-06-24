sudo su

firewall-cmd --list-all-zone

firewall-cmd --perment --new-zone=wifi

firewall-cmd --perment --new-zone=wifi --add-port=80/tcp

firewall-cmd --perment --new-zone=wifi --add-port=443/tcp

exit
