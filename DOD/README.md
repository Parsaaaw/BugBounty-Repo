# Description
This repository is a small part of DOD Recon.



## Important
> This repository does not have subdomains.



## Domain Discovery
### assetfinder
```
assetfinder .mil | anew mil.all
cat mil.all | grep -v "@" | grep -v  "=" | sed -e "s/\///g" | rev | cut -d "." -f 1,2 | rev | sort -u | anew domains.txt
```


### subfinder
```
subfinder -d .mil -all -silent | anew
```


### crt.sh
```
curl -sk "https://crt.sh/?q=%25.mil&output=json" | jq -r '.[].name_value' | sort -u | anew mil.all
cat mil.all | grep -v "@" | grep -v  "=" | grep "\." |  sed -e "s/\///g"  | rev | cut -d "." -f 1,2 | rev | sort -u | anew domains.txt
```

#### Manual
```
https://crt.sh/?O=Defense%20Media%20Activity%20(DMA)
https://crt.sh/?O=DoD%20Network%20Information%20Center
```



## Merge
```
cat mil.all | grep ".mil$" | grep -v "@" | grep -v  "=" | grep -vE "https?:" | sort -u | anew milSubdomains.txt
```



## Subdomain Discovery
### assetfinder
```
assetfinder .mil -subs-only | anew mil.all
```
```
cat domains.txt | while read subs;do assetfinder $subs -subs-only | anew milSubdomains.txt;done
```


### amass
```
amass enum --passive -df domains.txt -src -o amass
cat amass | awk '{print $2}' | sort -u | anew milSubdomains.txt
```


### subfinder
```
cat domains.txt | while read subs;do subfinder -d $subs -silent| anew milSubdomains.txt;done

cat domains.txt | while read subs;do subfinder -d $subs -all -silent| anew milSubdomains.txt;done
```



## RootSubdomain Discovery
### Manual
```
https://crt.sh/?q=%25.mil
```


### CLI
```
cat milSubdomains.txt | rev | cut -d "." -f 1,2,3 | rev | sort -u
```



## Extract IPs
```
cat mil.all | grep -oE '([0-9]{1,3}\.){3}[0-9]{1,3}' | anew IPs.txt
```