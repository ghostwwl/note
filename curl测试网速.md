

>curl -o /dev/null -s -w "\nnamelookup: "%{time_namelookup}"\nconnect: "%{time_connect}"\nstarttransfer: "%{time_starttransfer}"\ntotal: "%{time_total}"\nspeed: "%{speed_download}"\n" "http://www.baidu.com"
