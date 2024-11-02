# WordPress

## WP-Scan

```bash
# basic usage
wpscan --url "target" -o wpscan.info 

# enumerate vulnerable plugins, users, vulrenable themes, timthumbs
wpscan --url "target" --api-token "token" -o wpscan.info --enumerate vp,u,vt,tt 
```

Enumeration flags

```bash
# Usernames
--enumerate u 

# Vulnerable Plugins 
--enumerate vp

# Popular Plugins 
--enumerate p 

# All Plugins, aggressive detection
--enumerate ap --plugins-detection aggressive

# Vulnerable Themes 
--enumerate vt 

# All Themes 
--enumerate at 

# Popular Themes 
--enumerate t 

# Backups 
--enumerate cb 

# Database Exports 
--enumerate dbe
```
