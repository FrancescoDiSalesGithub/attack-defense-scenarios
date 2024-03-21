# Web Fuzzing

Usually a web fuzzing attack checks with the help of a dictionary if in the webserver there is a specific folder or file.

# Attacker side

From an attacker side it usually works like the following:

```

ffuf -w mydictionary:FUZZ -u http://victimip/FUZZ

```

Where victimip is the ip of the webserver where the fuzzing is starting.

# Defense side

On the other end suppose that we have a webserver like nginx, checking on access.log (it can be found at **/var/log/nginx/access.log**) we can find something like this:

```
192.168.1.2 - - [18/Mar/2024:18:59:57 +0000] "GET /13657 HTTP/1.1" 404 134 "-" "Fuzz Faster U Fool v2.1.0-dev"

```

The Fuzz Faster U Fool is the user agent of fuzz usually. In the next section we will use snort ids to check if someone is using fuzz against the webserver

## First Remediation

Let's write the following rule in our ruleset (it can be the local one or a custom one):

```

alert tcp any any <> any 80 (msg:"FUZZ tool is used against the webserver";content:"User-Agent: Fuzz Faster";sid:1001;rev:1);

```

Then let's run snort as the following:

```

sudo snort -c /path/to/local.rule -A console

```

With this command we can test if fuzz is working against our web server

## Hacking first remediation

The measure used above can be easily hacked by adding the parameter -H to fuzz:

```

ffuf -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-small.txt:FUZZ -H 'User-Agent:Firefox' -u http://192.168.1.3/FUZZ

```

# Adding additional defense by using splunk

To keep tracking of a possible attacker, we can use with snort, the tool splunk. In the following paragraph we will install a splunk universal forwarder on the server and create another server where splunk enterprise is installed.

## Splunk enterprise server install on the linux server

To download the splunk enterpise server, you can try two options:

* 1 - Registering on the splunk website and download the universal forwarder for linux
* 2 - Clone my github repository: 

After this, go to the bin folder and run splunk:

```

./splunk start

```

press the enter button, and then choose the password for your splunk server, and after go to:

```

http://localhost:8000

```

insert as username admin and as password the password you have just inserted in the previous step.

## Splunk universal forwarder install on the linux server

From the server where there is snort, you have to download the splunk universal forwarder, you can try two options:

* 1 - Registering on the splunk website and download the universal forwarder for linux
* 2 - Clone my github repository: 

After this, go to the bin folder and run splunk:

```

./splunk start

```

add the forwarder by using the following command:

```

./splunk add forwarder http://<IP_SPLUNK_SIEM>:9997

```

then add the deploy poll:


```

./splunk set deploy-poll http://<IP_SPLUNK_SIEM>:8089

```

add the nginx log file to monitor

```

./splunk add monitor /var/log/nginx

```


