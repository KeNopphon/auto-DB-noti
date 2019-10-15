# auto-DB-noti

**edx-discussion board crawler** is a Python-based Discussion board (DB) auto-notification tool developed by Tokyo TechX team to automatically and periodically notify the course staffs and lecturers on the incoming new posts and responses. Here we use crontab and rtcwake functionality of Ubuntu to control the scheduling and the transit to sleep mode respectively.  

## Prerequisites
Python libraries and modules:
* [Python](https://www.python.org/downloads/) - version 3.5+
* [Selenium with Python](https://selenium-python.readthedocs.io/) - API to access webdriver
* [Chrome webdriver](http://chromedriver.chromium.org/downloads) - Chrome webdriver which requires the installation of official Chrome brower.  
* [crontab](https://help.ubuntu.com/community/CronHowto) a system daemon used to execute desired tasks (in the background) at designated times.
* [rtc-wake](http://manpages.ubuntu.com/manpages/cosmic/man8/rtcwake.8.html) a command in Linux allowing a system to enter sleep state until specified wakeup time

## How to run
1) Enter edx account and host email information in 'account_info.json'
2) Enter a list of desired courses url in 'course table.xlsx' (column 1-2) 
3) Run a python script `edx_DBcrawler3.py` to initially scrape all discussion board textual data as well as mark all items as read

	python edx-discussion.py 

4) specify the subdirectory name (under 'HTMLs' foloder) of each desired courses in 'course table.xlsx' column 3   
5) add recipients email address information in 'course table.xlsx' column 4. 
  5.1)The format is [RECEPIENT NAME1],[RECEPIENT EMAIL1], [RECEPIENT FLAG1];[RECEPIENT NAME2],[RECEPIENT EMAIL2], [RECEPIENT FLAG2] 
  5.2) The FLAG is either 'yes' or 'no' indicating whether this specific recipient will receive the notification even there is no update of new activity in discussion at certain period or not
6) Modify the scheduling task in 'execute.sh' file
  6.1) repalce [WORKSPACE DIR] which directory of your workspace. For example, my workspace is in 'auto-db' folder then  [WORKSPACE DIR] --> home/user1/auto-db/
  6.2) If you do not intend to sleep the machine, please remove the last line and ignore the 6.3-6.4.
  6.3) if your account is not ROOT, change "PASSWORD" with your user password
  6.4) indicate the next wake up time by replacing [WAKEUP DATETIME] with the datetime format as explained in [here](http://manpages.ubuntu.com/manpages/xenial/man1/date.1.html) in case of UBUNTU user. for example, to wake up the machine every Monday 05:30 AM then [WAKEUP DATETIME] --> 'next Monday 05:30'

7) Setup the scheduling detail in crontab by typing a command. 
        crontab -e

8) modify the time scheduling to execute the 'execute.sh' file using crontab command. For example,   to execute every Monday 06:00 AM then
         0 6 * * MON ./[WORKSPACE DIR]/execute.sh
Please refer to the link in prerequisites for more detail.

9) DONE!!, you may execute the last line of 'execute.sh' in terminal to sleep the machine. If noting goes wrong, it will wake up and do the task in the next cycle.

To debug the program, you may see the log file located /tmp/crontest.log
The output of crawled textual DB data is is stored in "HTMLs\[COURSE_NAME]" folder .


## Test environment
Python 3.5, Windows 10, UBUNTU 14.04
