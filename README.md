# Cómo configurar conexión ssh a servidor
## Agregar conexión por ssh
### Para agregar conexión por ssh:
- Dirigirse a la carpeta ~/.ssh: 
```bash
cd ~/.ssh
```
- En el equipo local generar una nueva clave, si no ingresas nombre de archivo debería generar una por defecto con el nombre `id_rsa`: 
```bash
ssh-keygen -t rsa -b 4096
```

- Copiar la clave pública al servidor: 
```bash
# Si está por el puerto 22 por defecto:
ssh-copy-id root@211.11.11.111

# Si configuraste un puerto:
ssh-copy-id -p 2222 root@211.11.11.111
```

- En nuestro archivo local `~/.ssh/config` deberíamos tener algo como esto; (Ignorar github):
```yaml
# Si tienes configuración ssh con github:
Host *.github.com
  AddKeysToAgent yes
  UseKeychain yes
  IdentityFile ~/.ssh/current_github

# Agrega cuantos servidores desees, solo requieres duplicar las siguientes líneas, puedes usar el mismo IdentifyFile para todos los servidores:
Host my-server
  HostName 211.11.11.111  # Reemplaza con la dirección IP de tu servidor
  User root         # Reemplaza con tu nombre de usuario en el servidor
  Port 55555               # Reemplaza con el puerto que configuraste para SSH
  IdentityFile ~/.ssh/id_rsa  # Ruta a tu clave privada
```

# Verificar y configurar si el servidor tiene habilitadas las opciones para conectar por ssh:
- Ingresar al servidor:
```bash
ssh root@211.11.11.111
```

- En el servidor abrir este archivo: 
```bash
sudo nano /etc/ssh/sshd_config
```

- Asegurarse de que las siguientes líneas tengan la configuración a continuación y guardar cambios: 
```yaml
# Deshabilitar ingreso por puerto
PubkeyAuthentication yes
PasswordAuthentication no

# Cambiar puerto
Port 55555
```

- Reiniciar el servicio ssh: 
```bash
sudo systemctl restart ssh
```
- Si cambio el puerto ssh: 
```bash
sudo ufw allow 55555/tcp
```
- Si hago cambios en el firewall, recargar configuración: 
```bash
sudo ufw reload
```
- Si el firewall está inactivo: 
```bash
sudo ufw status 
#Activarlo: 
sudo ufw enable
```

- Activar los puertos para http y https:
```bash
sudo ufw allow 80/tcp     # Para HTTP
sudo ufw allow 443/tcp    # Para HTTPS$ sudo ufw reload
```

# Otros comandos útiles:
### Subir archivos:
```bash
# -r es para hacer una subida recursiva
# NOTA: Sube la carpeta con el nombre del directorio, no es necesario crear otra carpeta en el servidor
scp -r /my-local-folder my-server:/root
```

### Descargar:
```bash
# -r es para hacer una descarga recursiva
scp -r my-server:/root/file-o-foler my-local-folder
```
