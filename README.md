# recontools
instructions pour reconnaissance bug bounty


## recherche sous-domaine manuellement 

https://www.virustotal.com/gui/home/u
https://chaos.projectdiscovery.io/#/
https://crt.sh/?q=

## recherche sous-domaine automatisée

https://github.com/tomnomnom/assetfinder
https://github.com/owasp-amass/amass
https://github.com/projectdiscovery/subfinder

```bash
sudo subfinder -d nom-de-domaine.com -all > domaines1.txt
```

```bash
sudo assetfinder -subs-only nom-de-domaine.com > domaines2.txt
```

```bash
sort -u domaine1.txt domaines2.txt > domaines.txt
```

## recherche des domaines actifs 
### avec httpx :
```bash
cat domaines.txt | sudo httpx > domaines_actifs.txt
```

### avec subzy :
```bash
subzy run --targets domaines.txt
```

## utilisation de nuclei subdomain takeover
```bash
nuclei -l domaines.txt -t nuclei-templates/detect-all-takeovers.yaml

nuclei -l domaines_actifs.txt -t nuclei-templates/detect-all-takeovers.yaml
```

## url finder
```bash
katana -u http://sous-domaine.domaine-actif.com -o urls.txt

ou

katana -u http://domaine-actif.com -o urls.txt

# essayer des domaines jusqu'à trouver ou essayer autre outil

echo "https://domaine.com" | waybackurls > urls.txt

# recherche de paramètres dans les urls trouvées
cat urls.txt | grep =

cat urls.txt | grep ?*=

cat urls.txt | grep search

cat urls.txt | grep *?*=

# sinon on utilise arjun pour la recherche de paramètres
```
## OneForAll 
```bash
cd OneForAll
chmod +x oneforall.py

python3 oneforall.py --targets ../domaines.txt run
python3 oneforall.py --targets ../domaines_actifs.txt run
```

## google dorking
```bash
# google dorks sur telegram

# exemple recherche google :
intitle:"Index of" ".htpasswd" .htpasswd.bak site:domaine.com

filtetype:sql password site:domaine.com

# utilisation d'un moteur de recherche pour bug bounty
# https://nitinyadav00.github.io/Bug-Bounty-Search-Engine/
```
## github dorking
```bash
# github dorks sur telegram
# parfois les devs oublient des clefs d'API, des mdp par défaut dans github
# exemple recherche github :
"domaine.com" token

```

## nuclei template pour cves etc...
https://github.com/projectdiscovery/nuclei-templates

## tester sqlmap, xss-vibe ...

# liens utiles :

brute force subdomain = ffuf gobuseter dirbuster amass

wordlist = Seclists , https://github.com/n0kovo/n0kovo_subd...

live domain = httpx https://github.com/projectdiscovery/h...

screenshoting = gowitness https://github.com/sensepost/gowitness  utile pour prendre des screenshot depuis un CLI

deep recon = https://github.com/shmilylty/OneForAll

find urls = waybackurl katana https://github.com/projectdiscovery/k... , https://github.com/GerbenJavado/LinkF...

js data = subjs https://github.com/lc/subjs , katana -jc  

find path = dirsearch, ffuf https://github.com/ffuf/ffuf

parameter finder = arjun https://github.com/s0md3v/Arjun 

subdomain find = subzy https://github.com/PentestPad/subzy

Broken Link Hijacking = socialhunter https://github.com/utkusen/socialhunter  
```bash
socialhunter -f domaines_actifs.txt

```
payloads = https://github.com/swisskyrepo/PayloadsAllTheThings

port finder = nmap p -T4 -sC -sV [ip address]

google dorking = https://nitinyadav00.github.io/Bug-Bo...

xss = https://github.com/faiyazahmad07/xss_...
