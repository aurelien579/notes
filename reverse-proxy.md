## Transmission daemon

Il faut désactiver l'accès direct de l'extérieur au daemon.

Editer le fichier /etc/transmission-daemon/settings.json :
```json
{
    "rpc-whitelist": "127.0.0.1",
    "rpc-whitelist-enabled": true
}
```

Chargement de la nouvelle config :
```
invoke-rc.d transmission-daemon reload
```

## Génération du certificat auto-signé

```
openssl req -x509 -newkey rsa:4096 -nodes -days 365 \
    -out /etc/ssl/certs/transmission.crt \
    -keyout /etc/ssl/private/transmission.key
```

Clé privée : /etc/ssl/private/transmission.key
Certificat : /etc/ssl/certs/transmission.crt

Protection de la clé privée :
```
chmod 440 /etc/ssl/private/transmission.key
```

## Configuration d'apache

Il faut activer des modules apache:
```
a2enmod proxy proxy_http ssl
```

Les fichiers de configuration d'apache sont dans /etc/apache2/.

sites-enabled/000-default.conf
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
	SSLCertificateFile /etc/ssl/certs/transmission.crt
	SSLCertificateKeyFile /etc/ssl/private/transmission.key

    # Reverse proxy: *:443/transmission => localhost:9091/transmission
	<Location /transmission>
		ProxyPass http://localhost:9091/transmission
		ProxyPassReverse http://localhost:9091/transmission
	</Location>	
</VirtualHost>
```

Relancer apache2:
```
service apache2 restart
```

# OpenVPN

Si openvpn écoute déjà sur le port 443, il faut changer la config d'openvpn.

- /etc/openvpn/server.conf:
```
port 1194
```

```
service openvpn restart
```
