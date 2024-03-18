# Web Fuzzing

Usually a web fuzzing attack checks with the help of a dictionary if in the webserver there is a specific folder or file.

# Attacker side

From an attacker side it usually works like the following:

```

ffuf -w mydictionary:FUZZ -u http://victimip/FUZZ

```

Where victimip is the ip of the webserver where the fuzzing is starting.

# Defense side

On the other end suppose that we have a webserver like nginx, checking on access.log (it can be find at **/var/log/nginx/access.log**) we can find something like this:

```
192.168.1.2 - - [18/Mar/2024:18:59:57 +0000] "GET /13657 HTTP/1.1" 404 134 "-" "Fuzz Faster U Fool v2.1.0-dev"

```

The Fuzz Faster U Fool is the user agent of fuzz usually. In the next section we will use snort ids to check if someone is using fuzz against the webserver

## First Remediation

Let's write the following rule in our ruleset (it can be the local one or a custom one):

```

alert tcp any any <> any any (msg:"FUZZ tool is used against the webserver";content:"User-Agent: Fuzz Faster";sid:"1001";rev:1);

```

Then let's run snort as the following:

```

sudo snort -c /path/to/local.rule -A console

```

With this command we can test if fuzz is working against our web server

## Hacking first remediation

The measure used above can be easily hacked by adding the following parameter to fuzz:

```
TODO
```

