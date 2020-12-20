# Server-Pinger
Script to check if another server(s) is up.
  
  
```
#!/bin/bash
#
# ServerMonitor.sh
#
# This script monitors any number of external servers to try to determine if
# they are up and reachable.  I execute it every 5 minutes.  Place the foreign
# server(s) domain name or IP number in the ServerArray below.  To temporarily
# disable checking a server, place a # at the beginning of its line in the array.
#
# Cron: Cron: */5 * * * * /path/to/ServerMonitor.sh &> /dev/null
#

filePath=${0%/*}  # current file path

emailaddr="your@email.address"

ServerArray=(
"server1.tdl"
"server2.tdl"
"server3.tdl"
)

for index in ${!ServerArray[*]}; do
    cat /dev/null > $filePath/ServerMonitor.txt            # empty work file
    varserv=${ServerArray[index]}                          # get a server DN from ServerArray
    $(ping -c3 -w3 $varserv > $filePath/ServerMonitor.txt) # ping another server 3 times and timeout after 3 seconds

    if [[ $(grep -c "0 received" $filePath/ServerMonitor.txt) -gt 0 ]]; then
       mutt -s "Warning: Server $varserv may be down!" $emailaddr <<< "Warning: Server $varserv may be down!"
    fi
done

rm $filePath/ServerMonitor.txt                             # delete work file

exit 0
```
  
