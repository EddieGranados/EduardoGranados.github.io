## Ping Scan

```
# importing module      
import re       
import os              

# nmap command to perform a Ping Scan       
os.system('nmap -sn 192.168.1.* | cat > ping_scan.txt')             
        
# open and read file        
with open(r"ping_scan.txt") as file:        
    fileContent = file.readlines()      
    file.close      
        
# declaring regex pattern for ip addresses      
ip_pattern = re.compile(r'(\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3})')        
        
# initializing objects      
lst = []        
valid_list = ""     
invalid_list = ""       
        
# extracting the IP addresses       
for line in fileContent:        
    lst.append(ip_pattern.findall(line))        
        
# splitting empty cells from non-empty      
for i in range(0, len(lst)-1):      
    dirty_addressess = lst[i]       
    if len(dirty_addressess) == 0:      
        invalid_list = invalid_list+str(dirty_addressess)       
    else:       
        valid_list = valid_list+str(dirty_addressess)       
        
# removing special characters       
clean_ips = re.sub(r"[\[([{''})]", "", valid_list)      
clean_ips = re.sub(r"[\]]", "\n", clean_ips)        
        
# writing IPs to new file       
with open("outfile.txt",'w') as outfile:        
    for l in clean_ips:     
        outfile.write(l)        
    outfile.close()     
```

---

###### [Home](https://eddiegranados.github.io/Eduardo_Granados/)        