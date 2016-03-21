# Setting up a Cron Job on Ubuntu EC2 Instance

A cron job is most helpful when certain tasks need to repeat multiple times. This document is based on [Linux utility Cron 
to schedule your R script](https://help.ubuntu.com/community/CronHowto)

#### Installing cron

Log into Ubuntu EC2 instance through terminal. Install cron by typing: 

	sudo apt-get install gnome-schedule

#### Set up cron job

To create a cron job, add entries to your own user's crontab file. Start the crontab editor from a terminal window:

	crontab -e

Each of the sections is separated by a space, with the final section having one or more spaces in it. No spaces are allowed 
within Sections 1-5, only between them. Sections 1-5 are used to indicate when and how often you want the task to be 
executed. This is how a cron job is laid out:

	minute (0-59), hour (0-23, 0 = midnight), day (1-31), month (1-12), weekday (0-6, 0 = Sunday), command
	
After saving the edit file, a cron job is automatically set up to run. No other actions needed.

To test that cron is working properly. Do something like this :

	#Run an Rscript on our server and log the error messages
	*/1 * * * * Rscript /home/rstudio/test_cron.R >> /home/rstudio/testcron.log 2>&1
	


Here is a site that can help form cron commands in the crontab:

http://cron.nmonitoring.com/cron-generator.html

Other useful tutorials:

http://www.unixgeeks.org/security/newbie/unix/cron-1.html

https://tgmstat.wordpress.com/2013/09/11/schedule-rscript-with-cron/

http://nerdsrule.co/2013/09/03/scheduling-r-tasks-with-crontab-to-conserve-memory/

http://www.freebsd.org/cgi/man.cgi?crontab(5

http://www.thegeekstuff.com/2011/07/cron-every-5-minutes/

