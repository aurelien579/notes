# Gnome extensions

https://extensions.gnome.org/

Télécharger la bonne version de l'extension. Pour connaître la version de gnome:
```
gnome-shell --version
```
## Installer une extension en format .zip

#### Obtenir l'UUID de l'extension
```
unzip -c <filename>.zip metadata.json | grep uuid
```
#### Installation
```
mkdir -p ~/.local/share/gnome-shell/extensions/<UUID>
unzip -q <filename>.zip -d ~/.local/share/gnome-shell/extensions/<UUID>

gnome-shell-extension-tool -e <UUID>
```
