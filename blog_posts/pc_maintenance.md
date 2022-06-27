[Screenshot of result](https://github.com/EddieGranados/Eduardo_Granados/blob/gh-pages/blog_posts/Screenshots/pc_maintenance_2022-06-23.png)


```bash
#! /bin/bash


startTime=$(date) # Time program starts


# Displays to terminal
echo "****************************************************"
echo "Executing \"apt-updater.sh\""
echo "Starting @ $startTime"
echo "****************************************************"


echo "" > updater.log # Creates updater.log/Clears updater.log


# Appended to log
echo "****************************************************" >> updater.log 
echo "Executing \"apt-updater.sh\"" >> updater.log
echo "Starting update @ $startTime" >> updater.log
echo "****************************************************" >> updater.log 
echo "" >> updater.log


# Display to terminal
echo "."
sleep 3s # waits 3 seconds
echo "."


# Appended to log
sudo apt-get update >> updater.log &&
sudo apt-get dist-upgrade -Vy >> updater.log &&
sudo apt-get autoremove -y >> updater.log &&
sudo apt-get autoclean >> updater.log &&
sudo apt-get clean


endTime=$(date) # Time program ends


# Display to terminal
echo "."
sleep 3s
echo "."
sleep 3s


# Appended to log
echo "" >> updater.log 
echo "****************************************************" >> updater.log 
echo "Completed @ $endTime" >> updater.log
echo "****************************************************"  >> updater.log


# Displays to terminal
echo "****************************************************"
echo "Completed @ $endTime"
echo "****************************************************"
```