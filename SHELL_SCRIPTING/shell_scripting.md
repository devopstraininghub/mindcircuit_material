## **Basic Shell Scripting Questions**

**1. What is a shell script?**

A shell script is a text file containing a series of commands that the Unix/Linux shell interprets and executes sequentially.

---

**2. How do you make a shell script executable?**

```bash
chmod +x script.sh
./script.sh
```

---

**3. Difference between `sh` and `bash`?**

* `sh`: Bourne shell, minimal features.
* 
* `bash`: Bourne Again Shell, enhanced features (functions, arrays, arithmetic, better scripting).

---

**4. How do you pass arguments to a shell script?**

```bash
#!/bin/bash
echo "First arg: $1"
echo "Second arg: $2"
echo "Total args: $#"
echo "All args: $@"
```

---

**5. What is `$?` in shell scripting?**
It stores the **exit status** of the last executed command.

* `0` → success
  
* Non-zero → failure

---

## **Intermediate Shell Scripting Questions**

**6. How do you check if a file exists in shell script?**

```bash
if [ -f "/path/file.txt" ]; then
  echo "File exists"
else
  echo "File not found"
fi
```

---

**7. Difference between `>` and `>>` in shell scripting?**

* `>` → overwrite file
* `>>` → append to file

---

**8. How do you loop through a list of values?**

```bash
for i in 1 2 3 4; do
  echo "Number: $i"
done
```

---

**9. How do you check if two strings are equal in shell script?**

```bash
if [ "$str1" = "$str2" ]; then
  echo "Strings match"
fi
```

---

**10. How do you read input from a user in shell?**

```bash
read -p "Enter your name: " name
echo "Hello, $name"
```

---

## **Advanced/DevOps-Oriented Questions**

**11. How do you monitor CPU and memory usage using a shell script?**

```bash
#!/bin/bash
echo "CPU Usage:"
top -bn1 | grep "Cpu(s)"
echo "Memory Usage:"
free -m
```

---

**12. How do you find the top 5 processes consuming memory?**

```bash
ps -eo pid,ppid,cmd,%mem,%cpu --sort=-%mem | head -n 6
```

---

**13. How do you write a script to check if a service is running?**

```bash
#!/bin/bash
SERVICE="nginx"
if systemctl is-active --quiet $SERVICE; then
  echo "$SERVICE is running"
else
  echo "$SERVICE is not running"
fi
```

---

**14. How do you schedule a script to run every day at midnight?**
Use **cron job**:

```bash
crontab -e
0 0 * * * /path/to/script.sh
```

---

**15. How do you fetch metadata in an AWS EC2 instance using shell script?**

```bash
#!/bin/bash
curl -s http://169.254.169.254/latest/meta-data/public-ipv4
```

---

**16. How do you write a shell script for log rotation?**

```bash
#!/bin/bash
LOG_FILE="/var/log/app.log"
BACKUP_DIR="/var/log/backup"

mkdir -p $BACKUP_DIR
mv $LOG_FILE $BACKUP_DIR/app.log.$(date +%F)
touch $LOG_FILE
```

---

**17. How do you debug a shell script?**
Run with `-x`:

```bash
bash -x script.sh
```

---

**18. How do you store the output of a command in a variable?**

```bash
var=$(date)
echo "Current Date: $var"
```

---

**19. How do you find failed login attempts in Linux using a script?**

```bash
grep "Failed password" /var/log/auth.log | wc -l
```

---

**20. How do you write a script to check disk usage and send an alert if >80%?**

```bash
#!/bin/bash
THRESHOLD=80
USAGE=$(df -h / | awk 'NR==2 {print $5}' | sed 's/%//')

if [ $USAGE -gt $THRESHOLD ]; then
  echo "Disk usage is above $THRESHOLD%! Current: $USAGE%"
fi
```

Perfect 👌 Since you’re preparing as a **DevOps & Cloud Engineer**, let’s go deeper.

Here’s a **second set of shell scripting Q\&A** — focusing more on **real-time, tricky, and advanced interview scenarios**.

---

### **21. What are special variables in shell scripting?**

* `$0` → Script name
* `$1, $2…` → Positional arguments
* `$#` → Number of arguments
* `$@` → All arguments
* `$$` → Process ID of current script
* `$?` → Exit status of last command

---

### **22. How do you check if a variable is empty in shell script?**

```bash
if [ -z "$var" ]; then
  echo "Variable is empty"
else
  echo "Variable has value"
fi
```

---

### **23. Difference between `"$@"` and `"$*"`?**

* `"$@"` → treats each argument separately.
* `"$*"` → treats all arguments as one single string.

---

### **24. How do you use functions in shell scripting?**

```bash
my_function() {
  echo "Hello $1"
}
my_function "DevOps"
```

---

### **25. How do you trap signals (like Ctrl+C) in shell script?**

```bash
trap "echo 'Ctrl+C detected. Exiting safely...'; exit" SIGINT
while true; do
  echo "Running..."
  sleep 2
done
```

---

### **26. How do you replace a string in multiple files using shell?**

```bash
sed -i 's/old_text/new_text/g' *.txt
```

---

### **27. Write a script to ping a list of servers and report status.**

```bash
#!/bin/bash
for host in google.com amazon.com github.com; do
  if ping -c 1 $host &>/dev/null; then
    echo "$host is reachable"
  else
    echo "$host is not reachable"
  fi
done
```

---

### **28. How do you run a background job in shell?**

```bash
./script.sh &
jobs
fg %1   # bring back to foreground
```

---

### **29. What is `set -e`, `set -x`, and `set -u` in shell scripts?**

* `set -e` → Exit on error
* `set -x` → Print each command before execution (debug)
* `set -u` → Exit if using undefined variable

---

### **30. How do you find duplicate lines in a file?**

```bash
sort file.txt | uniq -d
```

---

### **31. Write a script to tail a log file and alert on "ERROR".**

```bash
#!/bin/bash
tail -Fn0 /var/log/app.log | \
while read line; do
  echo "$line" | grep "ERROR" && echo "ALERT: Error detected"
done
```

---

### **32. How do you use `here document` in shell scripting?**

```bash
cat <<EOF > file.txt
This is line 1
This is line 2
EOF
```

---

### **33. What’s the difference between `cron` and `at` command?**

* **cron** → Schedule repetitive jobs.
* **at** → Schedule one-time jobs.

---

### **34. How do you loop through files in a directory?**

```bash
for file in /path/*.log; do
  echo "Processing $file"
done
```

---

### **35. How do you check number of open file descriptors in Linux?**

```bash
lsof | wc -l
```

---

### **36. How do you write a script to restart a service if it fails?**

```bash
#!/bin/bash
SERVICE="nginx"
if ! systemctl is-active --quiet $SERVICE; then
  systemctl restart $SERVICE
  echo "$SERVICE restarted"
fi
```

---

### **37. How do you export variables for use in child processes?**

```bash
export VAR_NAME="DevOps"
```

---

### **38. What is the difference between `while` and `until` loop?**

* `while` → runs while condition is **true**.
* `until` → runs until condition becomes **true**.

---

### **39. How do you parse a CSV file line by line in shell script?**

```bash
while IFS=, read -r col1 col2 col3; do
  echo "Column1: $col1, Column2: $col2"
done < file.csv
```

---

### **40. How do you run the same command on multiple servers using shell script?**

```bash
for host in server1 server2 server3; do
  ssh $host "uptime"
done
```
