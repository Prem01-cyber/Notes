
- Mostly used in CTF's specifically in the wordpress sites.
- It looks for all the plugins in the wordpress site and checks if any of them are outdated and vulnerable and checks the actual theme and see how old it is.

```shell-session
wpscan --url http://tenet.htb -e ap --plugins-detection aggressive 
```

- `-e` indicates plugin check and `ap` indicates all plugins. We specify aggressive plugin detection. 