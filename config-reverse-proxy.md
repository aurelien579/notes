## Obtenir un certificat Let's encrypt

Attention il faut que le DNS ai les bonnes adresses ipv4 et ipv6 !

```
# Ajout du repot backports
echo 'deb http://deb.debian.org/debian stretch-backports main' >> /etc/apt/sources.list
apt update

# Installation de certbot
apt-get install certbot python-certbot-apache -t stretch-backports

# Certification
certbot --apache certonly
```

- Clé privée : `/etc/letsencrypt/live/nicmar.fr/privkey.pem`
- Certificat : `/etc/letsencrypt/live/nicmar.fr/fullchain.pem`


## Transmission daemon

Il faut désactiver l'accès direct de l'extérieur au daemon.

- `/etc/transmission-daemon/settings.json` :
```json
{
    "rpc-whitelist": "127.0.0.1",
    "rpc-whitelist-enabled": true
}
```

- Chargement de la nouvelle config :
```
invoke-rc.d transmission-daemon reload
```

## Configuration d'apache

- Activer des modules apache:
```
a2enmod proxy proxy_http ssl
```

Les fichiers de configuration d'apache sont dans `/etc/apache2/`.

- `sites-enabled/000-default.conf` :
```
# Active http pour servir le contenu du dossier /var/www/html
<VirtualHost *:80>
    DocumentRoot /var/www/html

    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>

# Active https
<VirtualHost *:443>
    DocumentRoot /var/www/html

    SSLEngine on
    SSLCertificateFile /etc/letsencrypt/live/nicmar.fr/fullchain.pem
    SSLCertificateKeyFile /etc/letsencrypt/live/nicmar.fr/privkey.pem

    # Reverse proxy: *:443/transmission => localhost:9091/transmission
    <Location /transmission>
        ProxyPass http://localhost:9091/transmission
        ProxyPassReverse http://localhost:9091/transmission
    </Location>	
</VirtualHost>
```

- Relancer apache2:
```
service apache2 restart
```

# OpenVPN

Si openvpn écoute déjà sur le port 443, il faut changer la config d'openvpn.

- /etc/openvpn/server.conf:
```
port 1194
```
- Relancer le service
```
service openvpn restart
```
