###### [Home](https://eddiegranados.github.io/Eduardo_Granados/)        

---

**For this program to work, you must have [Nmap](https://nmap.org/download.html) and [Python](https://www.python.org/downloads/) installed on your machine**
<br>
<br>



First we  import the [Regular Expression](https://docs.python.org/3/library/re.html#) and [Operating System](https://docs.python.org/3/library/os.html#) modules.
```bash
# importing module      
import re       
import os         
```
<br>



Next, the code will perform a ping scan using Nmap, and paste the results to a text document named `ping_scan.txt`. 
Using the `os.system(<command>)` we are able to execute a command in a terminal
```bash
# nmap command to perform a Ping Scan       
os.system('nmap -sn 192.168.1.* | cat > ping_scan.txt')
```
<br>



After the results of the ping scan have been added to a text file. The program will open and read the text document, then add its contexts the variable `fileContent`. The text file is then closed.
```bash
# open and read file        
with open(r"ping_scan.txt") as file:        
    fileContent = file.readlines()      
    file.close  
```
<br>



The variable `ip_pattern` is created, and this holds the pattern that will be used to match the IP address.
```bash
# declaring regex pattern for ip addresses      
ip_pattern = re.compile(r'(\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3})')
```
<br>



Three "list" are initialized. `lst` type is actually of `list`. `valid_list` and `invalid_list` are of type `String`.
```bash
# initializing objects      
lst = []        
valid_list = ""     
invalid_list = ""       
```
<br>



The program will loop through each line from `fileContent`. Then, append an IP address when a match for `ip_pattern` is found or an empty cell to `lst` .
```bash        
# extracting the IP addresses       
for line in fileContent:        
    lst.append(ip_pattern.findall(line))        
```
<br>



Program now separates the valid IPs from invalid IPs.
```bash        
# splitting empty cells from non-empty      
for i in range(0, len(lst)-1):      
    dirty_addressess = lst[i]       
    if len(dirty_addressess) == 0:      
        invalid_list = invalid_list+str(dirty_addressess)       
    else:       
        valid_list = valid_list+str(dirty_addressess)       
```
<br>



Since the values were a part of a type `list`, then changed to a `str`. The strings need to be cleaned, and have the special characters removed.
```bash    
# removing special characters       
clean_ips = re.sub(r"[\[([{''})]", "", valid_list)      
clean_ips = re.sub(r"[\]]", "\n", clean_ips)        
```
<br>



Lastly, the valid IPs are exported to a file named `outfile.txt`
```bash
# writing IPs to new file       
with open("outfile.txt",'w') as outfile:        
    for l in clean_ips:     
        outfile.write(l)        
    outfile.close()     
```
<br>



Full Code
```Python
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