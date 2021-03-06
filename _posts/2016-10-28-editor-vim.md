---
layout: post
title: vim
date: 2016-10-28
weather: cloudy
categories: Linux
tags: [Linux]
description:
---

# Shell

> Wormhole: [vagrant](https://www.vagrantup.com) & [PuPHPet](https://puphpet.com/#frontpage)

### 1 id & group

- [1] id
	- check whether the specified id exists

- [2] groups
	- [2.1]list groups OR check a specified group
	- [2.2]check whether a given user belong to a specified group

			=======================[1]================================
			id $UCD_USER > /dev/null 2>&1
			======================[2.1]===============================
			if [ "${p:agent/sys.os.name}" = "AIX" ]; then
				lsgroup $UCD_GROUP > /dev/null 2>&1
			else
				getent group $UCD_GROUP > /dev/null 2>&1
			fi
			======================[2.2]===============================
			id -nG "$IMP_USER" | grep -q "$UCD_GROUP"

---

### 2 Shell Pattern Matching
	- [shopt -s extglob](http://www.gnu.org/software/bash/manual/html_node/Pattern-Matching.html#Pattern-Matching)
	- [shopt: This builtin allows you to change additional shell optional behavior.](http://www.gnu.org/software/bash/manual/html_node/The-Shopt-Builtin.html)
	- [usage] shopt [-pqsu] [-o] [optname …]
		- "-s" Enable (set) each optname
		- "-u" Disable (unset) each optname.
		- "-q" Suppresses normal output
		- "-o" Restricts the values of optname to be those defined for the -o option to the set builtin

---

### 3 Set Command
	- set -o pipefail
		- If set, the return value of a pipeline is the value of the last (rightmost) command to exit with a  non-zero  status,or zero if all commands in the pipeline exit successfully.  This option is disabled by default.

		===========================3=================================
		#!/bin/bash

		ls ./a.txt |echo "hi" >/dev/null
		echo $? #0

		####################################
		set -o pipefail
		ls ./a.txt |echo "hi" >/dev/null
		echo $? #2

---

<!--
	>	[Quick Reference](http://www.adminschoice.com/crontab-quick-reference)
-->

> [Quick Reference] (https://help.ubuntu.com/community/CronHowto)

### 4 Crontab Job

- [Edit] crontab -e  
- [Change Editor] select-editor  
- [Test] Add new line: \*/2 \* \* \* \* date >> ~/time.log  
- [Start] sudo service cron restart  


				*     *     *   *    *        command to be executed
				-     -     -   -    -
				|     |     |   |    |
				|     |     |   |    +----- day of week (0 - 6) (Sunday=0)
				|     |     |   +------- month (1 - 12)
				|     |     +--------- day of        month (1 - 31)
				|     +----------- hour (0 - 23)
				+------------- min (0 - 59)

min |	hour	| day/month |	month	| day/week|	Execution time
---|---|---|---|---|
30	|0	|1	|1,6,12	|*	|— 00:30 Hrs  on 1st of Jan, June & Dec.
0	|20	|*	|10	|1-5	|–8.00 PM every weekday (Mon-Fri) only in Oct.
0	|0	|1,10,15	|*	|*	|— midnight on 1st ,10th & 15th of month
5,10	|0	|10	|*	|1	|— At 12.05,12.10 every Monday & on 10th of every month
\*/3,\*/5 | \*| \*| \*| \*  |— execute every 3 OR 5 mins, e.g. 10:03，10:05，10:06  

---

# 5 file/text Tools

- awk
	- [What is it]
		- Stands for Aho,Weinberger, Kernighan
		- An  AWK  program  is  a sequence of pattern {action} pairs and function definitions.
	- [Format] awk 'pattern + {action}'
		- [Note] One, but not both, of pattern {action} can be omitted.
	- [Action]
		- [Default Action] If {action} is omitted it is implicitly { print }.
		- [Short programs] are entered on the  command  line  usually enclosed  in ' ' to avoid shell interpretation.  
	    - [Longer programs] can be read in from a file with the -f option.
	- [Builtin-variables]
		- FS        splits records into fields as a regular expression.
		- NR        current record number in the total input stream.
		- NF        number of fields in the current record.
		- $0		current record
		- $1-$N 	fields 1-N of current record
	- [Built-in functions]
		- gsub(r,s): 		replace r with s (default t is $0)
		- index(s,t)：		return the first index # of s in t
		- length(s):
		- match(s,r): 		0 not match,  Returns the index of the first longest match
		- split(s,a,fs): 	String s is split into fields by REXPR r and the fields are loaded into array A.
		- substr(s,p):
	- [Program execution] BEGIN & END


			========================Example====================
			---------------------Default Root------------------
			  if [ -f /etc/redhat-release ]; then
			    APACHE_DEFAULT_DIR=$(grep -i '^DocumentRoot' /etc/httpd/conf/httpd.conf | awk '{print $2}' | sed sed 's/"//g')
			  elif [ $(uname) = "AIX" ]; then
			    APACHE_DEFAULT_DIR=$(grep -i '^DocumentRoot' /opt/freeware/etc/httpd/conf/httpd.conf | awk '{print $2}' | sed 's/"//g')
			  elif [ $(uname) = "Linux" ]; then
			    APACHE_DEFAULT_DIR=$(grep -i 'DocumentRoot' /etc/apache2/sites-available/\*default.conf | awk '{print $2}' | sed 's/"//g')
			  fi
			-----------------------Show lines------------------
			// show the line 3 - 5
			$ cat hello.txt | awk 'NR==13, NR==19{print $1,$NF}'
				<Directory />
				Options FollowSymLinks
				AllowOverride All
				</Directory> </Directory>

				<Directory /home/ray/www>
			-----------------------Show fields-----------------
			// show the first column of line 3 - 5
			$ cat hello.txt | awk 'NR==13, NR==19{print $1}'
				<Directory
				Options
				AllowOverride
				</Directory>

				<Directory
			-----------------[Program execution]---------------
			cat hello.txt | awk ' BEGIN { count=0; } { count+=length($0); } END { print count; }'
			-------------------GET Inline----------------------
			#!/bin/bash
			echo | awk '{ while(getline < "date.txt") { print $0; } }'
			------------------check dir empty------------------
			// Check if DIR is empty
			if [ $(ls -l "${DIR}" | awk 'NR==1 {print $2}') -eq 0 ]



---

# 6 Networks

- dig


---

# 7 compress & uncompress

				// uncompress
				sudo tar -zxvf v10.5fp6_linuxx64_server_t.tar.gz ./temp
				sudo tar -jxvf xx.tar.bz2

---

# 8 Install & Uninstall

- check installed app:
	- dpkg -l
- check app list
	- sudo apt-cache search all

---

# 9 environment

- User Env
	- ~/.profile
	- ~/.bash_profile OR ~./bash_login
	- ~/.bashrc

- System Env
	- /etc/environment
	- /etc/profile
	- /etc/bash.bashrc

---

# 10 remote tmux
ssh <remote host> -t tmux attach-session -t <session-name>


---
### Common Commands

- 1 Find Non-response App
	- ps -ef| grep skype
	- ps -aux| grep skype
- 2 Get Substring
	- expr substr 'this is an example' 3 4

---
