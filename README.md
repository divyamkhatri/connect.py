# connect.py
#ORIGINALLY MADE BY DIVYAM#

#IMPORT SECTION
import subprocess
import re
import json
import pickle
import time

#THIS PRINT ALL AVAILABLE NETWORKS ON YOUR INTERFACE
#THIS COMMAND IS USE HERE
#NETSH WLAN SHOW NETWORKS
info=subprocess.run(["netsh","wlan","show","networks"],capture_output=True).stdout.decode()
ssid=(re.findall("SSID (.*): (.*)\r",info))
available_net={}
for sid in ssid:
    ssid_no=(sid[0])
    ssid_name=(sid[1])
    available_net[ssid_name]=ssid_no
print("THESE ARE ALL AVAILABLE NETWORKS ON YOUR INTERFACE")
print(json.dumps(available_net,indent=1))
print("")
wifi=input("Type name of connection which u want to join :")
if wifi not in available_net:
    print("SORRY This network is not available on your interface try after refreshing")
    time.sleep(1)
else:
#THIS COMMAND IS USE HERE
#NETSH WLAN SET HOSTEDNETWORK MODE=ALLOW SSID=”YOUR WIFI CONNECTION NAME” KEY=”YOUR WIFI CONNECTION PASSWORD”
    wifi_key=input("enter the key:")
#THIS COMMAND IS USE HERE
#NETSH WLAN CONNECT <NETWORK_NAME> 
    subprocess.run(["netsh","wlan","set","hostednetwork mode=allow","SSID=",wifi,wifi_key])
    subprocess.run(["netsh","wlan","connect",wifi])
    print("conection succesful")
    print("quiting the program")
    time.sleep(0.5)
