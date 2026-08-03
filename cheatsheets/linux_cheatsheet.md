# Linux Cheatsheet

Guía rápida de comandos Linux organizada por secciones, con ejemplos prácticos y algunos comandos extra útiles listos para su uso. Además de secciones concretas acerca de administración de sistemas, automatización y troubleshooting.

## Índice

- [1. Navegación básica](#1-navegación-básica)
- [2. Gestión de archivos y directorios](#2-gestión-de-archivos-y-directorios)
- [3. Visualización y edición](#3-visualización-y-edición)
- [4. Búsqueda](#4-búsqueda)
- [5. Permisos y propiedad](#5-permisos-y-propiedad)
- [6. Usuarios y grupos](#6-usuarios-y-grupos)
- [7. Procesos y jobs](#7-procesos-y-jobs)
- [8. Monitorización del sistema](#8-monitorización-del-sistema)
- [9. Red](#9-red)
- [10. Disco, particiones y almacenamiento](#10-disco-particiones-y-almacenamiento)
- [11. Paquetes y repositorios](#11-paquetes-y-repositorios)
- [12. Compresión y archivos](#12-compresión-y-archivos)
- [13. Procesamiento de texto](#13-procesamiento-de-texto)
- [14. Shell scripting](#14-shell-scripting)
- [15. Systemd y servicios](#15-systemd-y-servicios)
- [16. DevOps y contenedores](#16-devops-y-contenedores)
- [17. SSH y transferencia](#17-ssh-y-transferencia)
- [18. Variables de entorno y shell](#18-variables-de-entorno-y-shell)
- [19. Comandos extra muy útiles](#19-comandos-extra-muy-útiles)
- [20. Atajos de terminal](#20-atajos-de-terminal)
- [21. Troubleshooting rápido](#21-troubleshooting-rápido)
- [22. Recomendaciones de uso](#22-recomendaciones-de-uso)
- [23. Comandos que conviene memorizar primero](#23-comandos-que-conviene-memorizar-primero)
- [24. Firewall básico](#24-firewall-básico)

## 1. Navegación básica

| Comando | Qué hace | Ejemplo |
|---|---|---|
| `pwd` | Muestra el directorio actual | `pwd` |
| `ls` | Lista archivos y directorios | `ls -la` |
| `cd` | Cambia de directorio | `cd /var/log` |
| `mkdir` | Crea un directorio | `mkdir proyecto` |
| `rmdir` | Borra un directorio vacío | `rmdir carpeta_vacia` |
| `tree` | Muestra estructura en árbol | `tree -L 2` |
| `clear` | Limpia la terminal | `clear` |

### Tips rápidos

- `cd ~` va al home.
- `cd -` vuelve al directorio anterior.
- `ls -lh` muestra tamaños legibles.
- `ls -ltr` ordena por fecha, dejando lo más reciente al final.

[↑ ir al índice](#índice)

## 2. Gestión de archivos y directorios

| Comando | Qué hace | Ejemplo |
|---|---|---|
| `touch` | Crea archivo vacío o actualiza timestamp | `touch app.log` |
| `cp` | Copia archivos o carpetas | `cp -r src/ backup/` |
| `mv` | Mueve o renombra | `mv viejo.txt nuevo.txt` |
| `rm` | Borra archivos | `rm archivo.txt` |
| `rm -r` | Borra directorios recursivamente | `rm -r carpeta/` |
| `rm -rf` | Fuerza borrado recursivo | `rm -rf tmp/` |
| `file` | Detecta tipo de archivo | `file imagen.iso` |
| `stat` | Muestra metadatos detallados | `stat script.sh` |
| `basename` | Obtiene nombre final de una ruta | `basename /etc/nginx/nginx.conf` |
| `dirname` | Obtiene directorio de una ruta | `dirname /etc/nginx/nginx.conf` |
| `ln -s` | Crea enlace simbólico | `ln -s /opt/app/current app` |

### Tips rápidos

- `cp -a` preserva permisos y atributos.
- `mv -i`, `cp -i` o `rm -i` piden confirmación antes de actuar.
- Mucho cuidado con `rm -rf`, especialmente al ejecutarlo como root.

[↑ ir al índice](#índice)

## 3. Visualización y edición

| Comando | Qué hace | Ejemplo |
|---|---|---|
| `cat` | Muestra archivo completo | `cat archivo.txt` |
| `less` | Navega por archivos largos | `less /var/log/syslog` |
| `head` | Muestra primeras líneas | `head -n 20 archivo.txt` |
| `tail` | Muestra últimas líneas | `tail -n 50 archivo.txt` |
| `tail -f` | Sigue cambios en tiempo real | `tail -f /var/log/nginx/access.log` |
| `nano` | Editor simple | `nano notas.txt` |
| `vim` | Editor avanzado | `vim docker-compose.yml` |
| `bat` | `cat` con resaltado, si está instalado | `bat script.py` |

### Atajos útiles en `less`

- `q`: salir.
- `/texto`: buscar hacia delante.
- `n`: siguiente coincidencia.
- `G`: ir al final.
- `g`: ir al inicio.

[↑ ir al índice](#índice)

## 4. Búsqueda

| Comando | Qué hace | Ejemplo |
|---|---|---|
| `find` | Busca archivos por nombre, tipo o tamaño | `find /var/log -name '*.log'` |
| `locate` | Búsqueda rápida en base de datos | `locate docker.sock` |
| `which` | Ruta de un ejecutable | `which python3` |
| `whereis` | Binario, source y man pages | `whereis nginx` |
| `grep` | Busca texto por patrón | `grep -R "listen" /etc/nginx` |
| `grep -i` | Ignora mayúsculas/minúsculas | `grep -i error app.log` |
| `grep -n` | Muestra número de línea | `grep -n TODO main.py` |
| `grep -v` | Excluye coincidencias | `grep -v '^#' .env` |
| `fd` | Alternativa moderna a `find`, si está instalada | `fd docker /etc` |
| `ripgrep` / `rg` | Búsqueda rápida en código | `rg "TODO\|FIXME" .` |

### Ejemplos prácticos

```bash
find . -type f -name "*.log"
find / -type f -size +500M 2>/dev/null
grep -R "password" /etc 2>/dev/null
rg "apiKey|token|secret" .
```

[↑ ir al índice](#índice)

## 5. Permisos y propiedad

| Comando | Qué hace | Ejemplo |
|---|---|---|
| `chmod` | Cambia permisos | `chmod 755 script.sh` |
| `chown` | Cambia propietario | `chown usuario:grupo archivo.txt` |
| `chgrp` | Cambia grupo | `chgrp developers app/` |
| `umask` | Define permisos por defecto | `umask 022` |
| `id` | Muestra UID, GID y grupos | `id usuario` |
| `whoami` | Usuario actual | `whoami` |
| `groups` | Grupos del usuario | `groups` |
| `getfacl` | Ver ACLs | `getfacl archivo.txt` |
| `setfacl` | Establecer ACLs | `setfacl -m u:deploy:rwx app.log` |

### Chuleta octal

| Valor | Permiso |
|---|---|
| `4` | Lectura |
| `2` | Escritura |
| `1` | Ejecución |
| `7` | `rwx` |
| `6` | `rw-` |
| `5` | `r-x` |
| `4` | `r--` |

### Ejemplos comunes

- `chmod +x script.sh`: hace ejecutable un script.
- `chmod -R 750 carpeta/`: aplica permisos recursivos.
- `chown -R www-data:www-data /var/www/app`: cambia dueño y grupo recursivamente.

[↑ ir al índice](#índice)

## 6. Usuarios y grupos

| Comando | Qué hace | Ejemplo |
|---|---|---|
| `useradd` | Crea usuario | `sudo useradd -m devops` |
| `usermod` | Modifica usuario | `sudo usermod -aG docker devops` |
| `userdel` | Elimina usuario | `sudo userdel -r temporal` |
| `passwd` | Cambia contraseña | `sudo passwd devops` |
| `groupadd` | Crea grupo | `sudo groupadd developers` |
| `groupdel` | Borra grupo | `sudo groupdel oldgroup` |
| `su -` | Cambia a otro usuario | `su - postgres` |
| `sudo` | Ejecuta como otro usuario/root | `sudo systemctl restart nginx` |
| `last` | Muestra últimos logins | `last` |
| `who` | Usuarios conectados | `who` |
| `w` | Quién está conectado y qué hace | `w` |

[↑ ir al índice](#índice)

## 7. Procesos y jobs

| Comando | Qué hace | Ejemplo |
|---|---|---|
| `ps aux` | Lista procesos | `ps aux \| grep nginx` |
| `top` | Monitor en tiempo real | `top` |
| `htop` | Versión mejorada de `top` | `htop` |
| `pgrep` | Busca PIDs por nombre | `pgrep -a python` |
| `pkill` | Mata procesos por patrón | `pkill -f uvicorn` |
| `kill` | Envía señal a un PID | `kill -15 1234` |
| `kill -9` | Fuerza terminación | `kill -9 1234` |
| `killall` | Mata por nombre de proceso | `killall firefox` |
| `jobs` | Lista trabajos de shell | `jobs` |
| `bg` | Reanuda en background | `bg %1` |
| `fg` | Trae job al foreground | `fg %1` |
| `nohup` | Deja proceso corriendo tras cerrar sesión | `nohup python app.py &` |
| `nice` | Lanza con prioridad distinta | `nice -n 10 comando` |
| `renice` | Cambia prioridad de proceso existente | `renice 5 -p 1234` |

### Señales comunes

| Señal | Significado |
|---|---|
| `-1` / `SIGHUP` | Reinicio de sesión / recarga |
| `-2` / `SIGINT` | Interrupción |
| `-9` / `SIGKILL` | Finalización forzada |
| `-15` / `SIGTERM` | Finalización ordenada |

[↑ ir al índice](#índice)

## 8. Monitorización del sistema

| Comando | Qué hace | Ejemplo |
|---|---|---|
| `uptime` | Carga media y uptime | `uptime` |
| `free -h` | Memoria RAM y swap | `free -h` |
| `vmstat 1` | Estadísticas de CPU, memoria e IO | `vmstat 1` |
| `iostat -xz 1` | IO y discos | `iostat -xz 1` |
| `mpstat -P ALL 1` | CPU por núcleo | `mpstat -P ALL 1` |
| `sar` | Métricas históricas | `sar -u 1 5` |
| `dmesg` | Mensajes del kernel | `dmesg \| tail -n 50` |
| `journalctl` | Logs de systemd | `journalctl -xe` |
| `watch` | Ejecuta repetidamente un comando | `watch -n 2 nvidia-smi` |
| `lsof` | Archivos abiertos / sockets | `lsof -i :8080` |
| `strace` | Traza syscalls | `strace -p 1234` |

### Casos útiles

```bash
journalctl -u nginx -n 100 --no-pager
watch -n 1 "df -h && free -h"
lsof /var/log/app.log
```

[↑ ir al índice](#índice)

## 9. Red

| Comando | Qué hace | Ejemplo |
|---|---|---|
| `ip a` | Muestra interfaces IP | `ip a` |
| `ip r` | Tabla de rutas | `ip r` |
| `ping` | Prueba conectividad | `ping 1.1.1.1` |
| `ss -tulpn` | Sockets y puertos abiertos | `ss -tulpn` |
| `netstat -tulpn` | Similar a `ss` en sistemas antiguos | `netstat -tulpn` |
| `curl` | Peticiones HTTP/API | `curl -I https://example.com` |
| `wget` | Descarga archivos | `wget https://example.com/file.iso` |
| `dig` | Consultas DNS | `dig example.com` |
| `nslookup` | Resolución DNS simple | `nslookup example.com` |
| `traceroute` | Ruta de red hasta destino | `traceroute 8.8.8.8` |
| `nc` | Netcat, prueba puertos y sockets | `nc -vz host 443` |
| `tcpdump` | Captura tráfico | `sudo tcpdump -i eth0 port 53` |
| `mtr` | Ping + traceroute continuo | `mtr 1.1.1.1` |

### Ejemplos prácticos

```bash
curl -s ifconfig.me
ss -tulpn | grep 8080
ip route get 8.8.8.8
dig +short google.com
```

[↑ ir al índice](#índice)

## 10. Disco, particiones y almacenamiento

| Comando | Qué hace | Ejemplo |
|---|---|---|
| `lsblk` | Lista discos y particiones | `lsblk -f` |
| `blkid` | UUID y tipo de FS | `blkid` |
| `df -h` | Uso de filesystem | `df -h` |
| `du -sh` | Tamaño de directorio | `du -sh /var/log` |
| `fdisk -l` | Tabla de particiones | `sudo fdisk -l` |
| `parted -l` | Info de particiones GPT/MBR | `sudo parted -l` |
| `mount` | Montajes actuales | `mount \| grep sda` |
| `umount` | Desmonta filesystem | `sudo umount /mnt/data` |
| `fsck` | Chequea/repara filesystem | `sudo fsck /dev/sdb1` |
| `mkfs.ext4` | Formatea ext4 | `sudo mkfs.ext4 /dev/sdb1` |
| `tune2fs` | Ajustes/info de ext filesystems | `sudo tune2fs -l /dev/sdb1` |
| `fstrim` | Lanza trim en SSD | `sudo fstrim -av` |

[↑ ir al índice](#índice)

## 11. Paquetes y repositorios

### Debian/Ubuntu

| Comando | Qué hace | Ejemplo |
|---|---|---|
| `apt update` | Refresca índice de paquetes | `sudo apt update` |
| `apt upgrade` | Actualiza paquetes instalados | `sudo apt upgrade` |
| `apt install` | Instala paquete | `sudo apt install nginx` |
| `apt remove` | Elimina paquete | `sudo apt remove nginx` |
| `apt purge` | Elimina paquete y configuración | `sudo apt purge nginx` |
| `apt search` | Busca paquete | `apt search docker` |
| `apt show` | Muestra info del paquete | `apt show curl` |
| `dpkg -l` | Lista paquetes instalados | `dpkg -l \| grep nginx` |

### Otras distros

| Gestor | Instalar |
|---|---|
| `dnf` | `sudo dnf install nginx` |
| `yum` | `sudo yum install nginx` |
| `pacman` | `sudo pacman -S nginx` |
| `zypper` | `sudo zypper install nginx` |
| `apk` | `sudo apk add nginx` |
| `snap` | `sudo snap install code --classic` |
| `flatpak` | `flatpak install flathub org.mozilla.firefox` |

[↑ ir al índice](#índice)

## 12. Compresión y archivos

| Comando | Qué hace | Ejemplo |
|---|---|---|
| `tar -cvf` | Crea tar | `tar -cvf backup.tar directorio/` |
| `tar -xvf` | Extrae tar | `tar -xvf backup.tar` |
| `tar -czvf` | Crea tar.gz | `tar -czvf backup.tar.gz directorio/` |
| `tar -xzvf` | Extrae tar.gz | `tar -xzvf backup.tar.gz` |
| `tar -cjvf` | Crea tar.bz2 | `tar -cjvf backup.tar.bz2 directorio/` |
| `zip` | Comprime en zip | `zip -r backup.zip directorio/` |
| `unzip` | Extrae zip | `unzip backup.zip` |
| `gzip` | Comprime archivo | `gzip archivo.log` |
| `gunzip` | Descomprime gzip | `gunzip archivo.log.gz` |
| `xz` | Compresión alta | `xz archivo.img` |
| `7z` | Formato 7zip | `7z a backup.7z directorio/` |

[↑ ir al índice](#índice)

## 13. Procesamiento de texto

| Comando | Qué hace | Ejemplo |
|---|---|---|
| `grep` | Busca patrones | `grep ERROR app.log` |
| `sed` | Sustitución y edición por stream | `sed 's/dev/prod/g' archivo.txt` |
| `awk` | Procesado por columnas/patrones | `awk '{print $1, $3}' archivo.txt` |
| `sort` | Ordena líneas | `sort usuarios.txt` |
| `uniq` | Elimina duplicados consecutivos | `sort usuarios.txt \| uniq` |
| `cut` | Extrae columnas | `cut -d':' -f1 /etc/passwd` |
| `tr` | Traduce caracteres | `echo hola \| tr a-z A-Z` |
| `wc` | Cuenta líneas, palabras, bytes | `wc -l archivo.txt` |
| `paste` | Une líneas por columnas | `paste a.txt b.txt` |
| `column` | Formatea salida tabular | `column -t -s, archivo.csv` |
| `jq` | Procesa JSON | `cat data.json \| jq '.items[]'` |
| `yq` | Procesa YAML | `yq '.services.web.image' docker-compose.yml` |

### One-liners útiles

```bash
ps aux | sort -rk 4 | head
journalctl -u ssh --since today | grep Failed
cut -d: -f1 /etc/passwd | sort
cat access.log | awk '{print $1}' | sort | uniq -c | sort -nr | head
```

[↑ ir al índice](#índice)

## 14. Shell scripting

| Elemento | Ejemplo |
|---|---|
| Shebang | `#!/usr/bin/env bash` |
| Variable | `NOMBRE="Linux"` |
| Lectura | `read -r nombre` |
| Condición | `if [[ -f archivo ]]; then ... fi` |
| Bucle `for` | `for f in *.log; do echo "$f"; done` |
| Bucle `while` | `while read -r line; do ...; done < archivo.txt` |
| Función | `mi_funcion() { echo "ok"; }` |
| Código de salida | `exit 0` |
| Debug | `bash -x script.sh` |

### Plantilla mínima

```bash
#!/usr/bin/env bash
set -euo pipefail

main() {
  echo "Hola desde Bash"
}

main "$@"
```

### Buenas prácticas

- Conviene usar `set -euo pipefail` en scripts serios.
- Las variables se citan con `"$VAR"`.
- Se prefiere `$(comando)` frente a backticks.
- Las dependencias se validan con `command -v`.

[↑ ir al índice](#índice)

## 15. Systemd y servicios

| Comando | Qué hace | Ejemplo |
|---|---|---|
| `systemctl status` | Estado de un servicio | `systemctl status nginx` |
| `systemctl start` | Arranca servicio | `sudo systemctl start nginx` |
| `systemctl stop` | Detiene servicio | `sudo systemctl stop nginx` |
| `systemctl restart` | Reinicia servicio | `sudo systemctl restart nginx` |
| `systemctl reload` | Recarga configuración | `sudo systemctl reload nginx` |
| `systemctl enable` | Activa al arranque | `sudo systemctl enable nginx` |
| `systemctl disable` | Desactiva del arranque | `sudo systemctl disable nginx` |
| `systemctl list-units --type=service` | Lista servicios | `systemctl list-units --type=service` |
| `journalctl -u` | Logs del servicio | `journalctl -u nginx -f` |

### Ejemplo de unidad simple

```ini
[Unit]
Description=Mi aplicación
After=network.target

[Service]
ExecStart=/usr/bin/python3 /opt/app/app.py
Restart=always
User=www-data

[Install]
WantedBy=multi-user.target
```

[↑ ir al índice](#índice)

## 16. DevOps y contenedores

| Comando | Qué hace | Ejemplo |
|---|---|---|
| `docker ps` | Lista contenedores | `docker ps` |
| `docker images` | Lista imágenes | `docker images` |
| `docker logs -f` | Sigue logs | `docker logs -f web` |
| `docker exec -it` | Entra en contenedor | `docker exec -it web bash` |
| `docker inspect` | Inspecciona recurso | `docker inspect web` |
| `docker compose up -d` | Levanta stack | `docker compose up -d` |
| `docker compose logs -f` | Logs del stack | `docker compose logs -f` |
| `docker compose down` | Baja stack | `docker compose down` |
| `kubectl get pods` | Lista pods | `kubectl get pods -A` |
| `kubectl describe pod` | Describe pod | `kubectl describe pod mi-pod` |
| `kubectl logs -f` | Logs de pod | `kubectl logs -f deployment/api` |
| `kubectl exec -it` | Shell en pod | `kubectl exec -it mi-pod -- sh` |

[↑ ir al índice](#índice)

## 17. SSH y transferencia

| Comando | Qué hace | Ejemplo |
|---|---|---|
| `ssh` | Accede a host remoto | `ssh usuario@host` |
| `scp` | Copia por SSH | `scp archivo.txt usuario@host:/tmp/` |
| `rsync` | Sincroniza archivos | `rsync -avz ./ usuario@host:/srv/app/` |
| `sftp` | Transferencia interactiva | `sftp usuario@host` |
| `ssh-copy-id` | Copia clave pública | `ssh-copy-id usuario@host` |

### Ejemplos interesantes

```bash
ssh -p 2222 usuario@host
rsync -avz --delete ./build/ usuario@host:/var/www/html/
scp -r proyecto/ usuario@host:/opt/
```

[↑ ir al índice](#índice)

## 18. Variables de entorno y shell

| Comando | Qué hace | Ejemplo |
|---|---|---|
| `env` | Muestra variables de entorno | `env` |
| `printenv` | Muestra variables | `printenv HOME` |
| `export` | Exporta variable | `export APP_ENV=prod` |
| `unset` | Elimina variable | `unset APP_ENV` |
| `alias` | Crea alias | `alias ll='ls -lah'` |
| `history` | Historial de comandos | `history \| tail` |
| `source` | Recarga archivo en shell actual | `source ~/.bashrc` |

[↑ ir al índice](#índice)

## 19. Comandos extra muy útiles

| Comando | Uso recomendado | Ejemplo |
|---|---|---|
| `xargs` | Pasar salida como argumentos | `find . -name '*.log' \| xargs rm -f` |
| `watch` | Repetir comando periódicamente | `watch -n 1 sensors` |
| `timeout` | Limitar duración de un comando | `timeout 10 ping 8.8.8.8` |
| `tee` | Ver y guardar salida a la vez | `ls -la \| tee listado.txt` |
| `sort -h` | Orden humano por tamaños | `du -sh * \| sort -h` |
| `comm` | Comparar archivos ordenados | `comm a.txt b.txt` |
| `diff -u` | Ver diferencias | `diff -u old.conf new.conf` |
| `md5sum` / `sha256sum` | Checksums | `sha256sum imagen.iso` |
| `realpath` | Ruta absoluta canónica | `realpath archivo.txt` |
| `seq` | Genera secuencias | `seq 1 10` |
| `yes` | Repite texto continuamente | `yes \| head` |

[↑ ir al índice](#índice)

## 20. Atajos de terminal

| Atajo | Acción |
|---|---|
| `Ctrl + C` | Interrumpe proceso actual |
| `Ctrl + Z` | Suspende proceso |
| `Ctrl + D` | Cierra shell / EOF |
| `Ctrl + L` | Limpia pantalla |
| `Ctrl + A` | Inicio de línea |
| `Ctrl + E` | Final de línea |
| `Ctrl + R` | Busca en historial |
| `!!` | Repite último comando |
| `!n` | Repite comando n del historial |
| `!cadena` | Repite último que empieza por cadena |

[↑ ir al índice](#índice)

## 21. Troubleshooting rápido

### Ver qué ocupa espacio

```bash
df -h
du -sh /* 2>/dev/null | sort -h
du -sh /var/* 2>/dev/null | sort -h
```

### Ver qué proceso usa un puerto

```bash
ss -tulpn | grep :8080
lsof -i :8080
```

### Seguir logs de un servicio

```bash
journalctl -u nginx -f
tail -f /var/log/nginx/error.log
```

### Probar red y DNS

```bash
ping -c 4 1.1.1.1
ping -c 4 google.com
dig google.com
curl -I https://example.com
```

### Buscar archivos grandes

```bash
find / -type f -size +1G 2>/dev/null
```

[↑ ir al índice](#índice)

## 22. Recomendaciones de uso

- Empezar por versiones seguras: `cp -i`, `mv -i`, `rm -i`.
- Usar `man comando` o `comando --help` para profundizar.
- Encadenar comandos con pipes para explotar la shell al máximo.
- Guardar aliases y funciones útiles en `~/.bashrc` o `~/.zshrc`.
- Para administración moderna, merece la pena dominar bien `ss`, `journalctl`, `find`, `grep`, `awk`, `sed`, `curl`, `jq`, `rsync` y `systemctl`.

[↑ ir al índice](#índice)

## 23. Comandos que conviene memorizar primero

```bash
ls -la
cd -
find . -name "*.log"
grep -R "texto" .
ss -tulpn
journalctl -xe
systemctl status servicio
df -h
free -h
du -sh *
tail -f archivo.log
curl -I https://example.com
rsync -avz origen/ destino/
chmod +x script.sh
ps aux | grep proceso
```

[↑ ir al índice](#índice)

## 24. Firewall básico

| Comando      | Qué hace       | Ejemplo    |
| ----- | ----- | ----- |
| ufw status verbose  | Muestra estado y reglas      | sudo ufw status verbose  |
| ufw enable   | Activa el firewall    | sudo ufw enable   |
| ufw disable  | Desactiva el firewall | sudo ufw disable  |
| ufw default deny incoming  | Bloquea entradas por defecto | sudo ufw default deny incoming  |
| ufw default allow outgoing | Permite salidas por defecto  | sudo ufw default allow outgoing |
| ufw allow ssh       | Permite SSH por perfil       | sudo ufw allow ssh       |
| ufw allow 22/tcp    | Permite un puerto TCP | sudo ufw allow 22/tcp    |
| ufw allow 443/tcp   | Permite HTTPS  | sudo ufw allow 443/tcp   |
| ufw allow 51820/udp | Permite WireGuard     | sudo ufw allow 51820/udp |
| ufw deny from 203.0.113.45 | Bloquea una IP | sudo ufw deny from 203.0.113.45 |
| ufw delete allow 22/tcp    | Borra una regla       | sudo ufw delete allow 22/tcp    |
| ufw app list | Lista perfiles de apps       | sudo ufw app list |
| ufw logging medium  | Activa logs    | sudo ufw logging medium  |

Ejemplo rápido

```bash
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow OpenSSH
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
sudo ufw enable
sudo ufw status verbose
``` 