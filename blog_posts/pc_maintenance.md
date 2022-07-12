###### [Home](https://eddiegranados.github.io/Eduardo_Granados/)

---


- I have created this script to help users keep their Linux machine that are sudo users and use apt-get to stay up-to date.
- There is for sure some extra lines of code that can be removed, like all the ones under a "Displays to terminal" comment. Everything else is what really matters.
- Check out how to set this up as a root user and have it run automatically as a [cron job](/blog_posts/cron_jobs.md).
- The `apt` command is a part of the package management software also named apt. Apt contains a whole suite of tools that allows us to manage the packages and sources of our software, and to install or remove software at the same time.



[Screenshot of result](https://github.com/EddieGranados/Eduardo_Granados/blob/gh-pages/blog_posts/Screenshots/pc_maintenance_2022-06-23.png)


```bash
#! /bin/bash


startTime=$(date) # Time program starts



# Displays to terminal
echo "****************************************************"
echo "Executing \"apt-updater.sh\""
echo "Starting @ $startTime"
echo "****************************************************"


echo "" > /home/eg/scripts/updater/updater.log # Creates updater.log/Clears updater.log


logPath="/home/eg/scripts/updater/updater.log" # It's easier to spell logPath than the entire direct path


# Appended to log
echo "****************************************************" >> $logPath
echo "Executing \"apt-updater.sh\"" >> $logPath
echo "Starting update @ $startTime" >> $logPath
echo "****************************************************" >> $logPath
echo "" >> $logPath


# Display to terminal
echo "."
sleep 3s # waits 3 seconds
echo "."


# Appended to log
sudo apt-get update >> $logPath &&
sudo apt-get dist-upgrade -Vy >> $logPath &&
sudo apt-get autoremove -y >> $logPath &&
sudo apt-get autoclean >> $logPath &&
sudo apt-get clean >> $logPath


endTime=$(date) # Time program ends


# Display to terminal
echo "."
sleep 3s
echo "."
sleep 3s


# Appended to log
echo "" >> $logPath 
echo "****************************************************" >> $logPath 
echo "Completed @ $endTime" >> $logPath
echo "****************************************************"  >> $logPath


# Displays to terminal
echo "****************************************************"
echo "Completed @ $endTime"
echo "****************************************************"
```