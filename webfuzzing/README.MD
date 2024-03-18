# Web Fuzzing

Usually a web fuzzing attack checks with the help of a dictionary if in the webserver there is a specific folder or file.

# Attacker side

From an attacker side it usually works like the following:

```

ffuf -w mydictionary:FUZZ -u http://victimip/FUZZ

```

Where victimip is the ip of the webserver where the fuzzing is starting.

# Defense side

On the other end suppose that we have a webserver like nginx, checking on access.log we can find something like this:

```
TODO
```
