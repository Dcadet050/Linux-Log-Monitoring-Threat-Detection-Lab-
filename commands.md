# Commands Used in Lab:

# 1. Splunk Server Setup

- wget -O splunk-10.2.1-c892b66d163d-linux-amd64.tgz "https://download.splunk.com/products/splunk/releases/10.2.1/linux/splunk-10.2.1-c892b66d163d-linux-amd64.tgz"
- tar -xvf splunk-10.2.1-c892b66d163d-linux-amd64.tgz -C /opt
- sudo /opt/splunk/bin/splunk start --accept-license --run-as-root

# 2. Ubuntu Victim Setup (Splunk Forwarder)

- wget -O splunkforwarder-10.2.1-c892b66d163d-linux-amd64.deb "https://download.splunk.com/products/universalforwarder/releases/10.2.1/linux/splunkforwarder-10.2.1-c892b66d163d-linux-amd64.deb"
- sudo dpkg -i splunkforwarder-10.2.1-c892b66d163d-linux-amd64.deb
- sudo /opt/splunkforwarder/bin/splunk add forward-server 192.168.56.2:9997
- sudo /opt/splunkforwarder/bin/splunk add monitor /var/log
- sudo /opt/splunkforwarder/bin/splunk restart

# 3. Generating Logs for Testing

- sudo apt update
- logger "SOC Lab test log message"
- ssh wronguser@localhost

# 4. Splunk Search Queries (SPL)

- index=main
- index=main sourcetype=syslog "Failed password"
- index=main sourcetype=syslog "Failed password" | stats count by host
- index=main sourcetype=syslog "Failed password" | timechart span=1h count

