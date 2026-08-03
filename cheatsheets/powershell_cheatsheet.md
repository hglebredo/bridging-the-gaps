# PowerShell Cheatsheet

Guía rápida de PowerShell organizada por secciones, con enfoque en cmdlets modernos y evitando comandos CMD/DOS cuando exista equivalente en PowerShell.

## Índice

- [1. Ayuda y descubrimiento](#1-ayuda-y-descubrimiento)
- [2. Navegación y ubicación](#2-navegación-y-ubicación)
- [3. Archivos y carpetas](#3-archivos-y-carpetas)
- [4. Búsqueda y filtrado](#4-búsqueda-y-filtrado)
- [5. Procesos y servicios](#5-procesos-y-servicios)
- [6. Red y conectividad](#6-red-y-conectividad)
- [7. Sistema e inventario](#7-sistema-e-inventario)
- [8. Archivos y hash](#8-archivos-y-hash)
- [9. Compresión y archivado](#9-compresión-y-archivado)
- [10. Variables y shell](#10-variables-y-shell)
- [11. Scripts y control](#11-scripts-y-control)
- [12. Automatización remota](#12-automatización-remota)
- [13. Seguridad y permisos](#13-seguridad-y-permisos)
- [14. Paquetes y módulos](#14-paquetes-y-módulos)
- [15. Formato y exportación](#15-formato-y-exportación)
- [16. Acciones frecuentes rápidas](#16-acciones-frecuentes-rápidas)
- [17. Comandos que conviene memorizar primero](#17-comandos-que-conviene-memorizar-primero)

## 1. Ayuda y descubrimiento

| Comando | Qué hace | Ejemplo |
|---|---|---|
| `Get-Help` | Muestra ayuda de un cmdlet | `Get-Help Get-ChildItem` |
| `Get-Command` | Busca comandos, alias y funciones | `Get-Command *service*` |
| `Get-Member` | Muestra propiedades y métodos de objetos | `Get-Process \| Get-Member` |
| `Get-Alias` | Lista alias disponibles | `Get-Alias ls` |
| `Update-Help` | Actualiza la ayuda local | `Update-Help` |

### Tip rápido

- Usar `Get-Help comando -Examples` para ver ejemplos.
- Usar `Get-Help comando -Full` para detalle completo.
- En PowerShell, casi todo devuelve objetos, no solo texto.

## 2. Navegación y ubicación

| Comando | Qué hace | Ejemplo |
|---|---|---|
| `Get-Location` | Muestra la ruta actual | `Get-Location` |
| `Set-Location` | Cambia de directorio | `Set-Location C:\Windows` |
| `Push-Location` | Guarda ubicación actual y cambia | `Push-Location C:\Temp` |
| `Pop-Location` | Vuelve a la ubicación previa | `Pop-Location` |
| `Get-ChildItem` | Lista archivos y carpetas | `Get-ChildItem -Force` |
| `Resolve-Path` | Resuelve ruta absoluta | `Resolve-Path .archivo.txt` |
| `Split-Path` | Divide una ruta | `Split-Path C:\Tempa.txt -Parent` |
| `Join-Path` | Une rutas | `Join-Path C:\Temp a.txt` |

### Equivalencias útiles

- `Get-ChildItem` es el equivalente moderno de `dir`.
- `Set-Location` es el equivalente moderno de `cd`.

## 3. Archivos y carpetas

| Comando | Qué hace | Ejemplo |
|---|---|---|
| `New-Item` | Crea archivo o carpeta | `New-Item -ItemType Directory -Path .\logs` |
| `Copy-Item` | Copia archivos o carpetas | `Copy-Item .a.txt D:\Backup\` |
| `Move-Item` | Mueve o renombra | `Move-Item .a.txt .b.txt` |
| `Remove-Item` | Borra archivos o carpetas | `Remove-Item .	emp -Recurse -Force` |
| `Rename-Item` | Renombra elemento | `Rename-Item .viejo.txt nuevo.txt` |
| `Test-Path` | Comprueba si existe | `Test-Path .\config.json` |
| `Get-Item` | Obtiene un elemento concreto | `Get-Item .\config.json` |
| `Get-Content` | Lee contenido de un archivo | `Get-Content .app.log -Tail 20` |
| `Set-Content` | Escribe contenido nuevo | `Set-Content .\hola.txt 'Hola'` |
| `Add-Content` | Añade contenido al final | `Add-Content .app.log 'Nueva línea'` |
| `Clear-Content` | Vacía el contenido | `Clear-Content .app.log` |
| `Out-File` | Redirige salida a archivo | `Get-Process \| Out-File .\procesos.txt` |

### Ejemplos prácticos

```powershell
New-Item -ItemType Directory -Path .\proyecto
New-Item -ItemType File -Path .
otas.txt
Copy-Item .\origen.txt .backupRemove-Item .\cache -Recurse -Force
Get-Content .\log.txt -Tail 50 -Wait
```

## 4. Búsqueda y filtrado

| Comando | Qué hace | Ejemplo |
|---|---|---|
| `Select-String` | Busca texto en archivos o entrada | `Select-String -Path .\*.log -Pattern 'error'` |
| `Where-Object` | Filtra objetos por condición | `Get-Process \| Where-Object CPU -gt 10` |
| `Sort-Object` | Ordena objetos | `Get-Process \| Sort-Object CPU -Descending` |
| `Select-Object` | Selecciona propiedades | `Get-Process \| Select-Object Name, CPU` |
| `Measure-Object` | Cuenta, suma, promedio, min/max | `Get-Content .a.txt \| Measure-Object -Line` |
| `Compare-Object` | Compara dos colecciones | `Compare-Object (Get-Content a.txt) (Get-Content b.txt)` |

### Ejemplos útiles

```powershell
Get-ChildItem -Recurse -File | Where-Object Length -gt 1GB
Get-Process | Sort-Object WorkingSet64 -Descending | Select-Object -First 10
Get-Content .app.log | Select-String -Pattern 'WARN|ERROR'
```

## 5. Procesos y servicios

| Comando | Qué hace | Ejemplo |
|---|---|---|
| `Get-Process` | Lista procesos | `Get-Process` |
| `Stop-Process` | Termina un proceso | `Stop-Process -Name notepad -Force` |
| `Start-Process` | Inicia un proceso | `Start-Process notepad.exe` |
| `Wait-Process` | Espera a que termine | `Wait-Process -Name chrome` |
| `Get-Service` | Lista servicios | `Get-Service` |
| `Start-Service` | Arranca servicio | `Start-Service Spooler` |
| `Stop-Service` | Detiene servicio | `Stop-Service Spooler` |
| `Restart-Service` | Reinicia servicio | `Restart-Service Spooler` |
| `Set-Service` | Cambia configuración del servicio | `Set-Service Spooler -StartupType Automatic` |

### Ejemplos prácticos

```powershell
Get-Process | Sort-Object CPU -Descending | Select-Object -First 5
Get-Service | Where-Object Status -eq 'Running'
Restart-Service W32Time
Stop-Process -Id 1234 -Force
```

## 6. Red y conectividad

| Comando | Qué hace | Ejemplo |
|---|---|---|
| `Test-Connection` | Prueba conectividad | `Test-Connection google.com -Count 4` |
| `Test-NetConnection` | Diagnóstico de red y puertos | `Test-NetConnection 8.8.8.8 -Port 443` |
| `Get-NetIPAddress` | Muestra IPs del sistema | `Get-NetIPAddress` |
| `Get-NetIPConfiguration` | Resumen de red | `Get-NetIPConfiguration` |
| `Get-NetAdapter` | Lista adaptadores | `Get-NetAdapter` |
| `Get-NetTCPConnection` | Conexiones TCP | `Get-NetTCPConnection` |
| `Resolve-DnsName` | Consulta DNS moderna | `Resolve-DnsName example.com` |
| `Get-NetFirewallRule` | Lista reglas de firewall | `Get-NetFirewallRule` |
| `Get-NetFirewallProfile` | Estado de perfiles firewall | `Get-NetFirewallProfile` |

### Ejemplos prácticos

```powershell
Test-NetConnection localhost -Port 3389
Get-NetIPAddress | Where-Object AddressFamily -eq IPv4
Get-NetTCPConnection | Where-Object State -eq Established
Resolve-DnsName microsoft.com
```

## 7. Sistema e inventario

| Comando | Qué hace | Ejemplo |
|---|---|---|
| `Get-ComputerInfo` | Información general del sistema | `Get-ComputerInfo` |
| `Get-CimInstance` | Consulta WMI/CIM moderna | `Get-CimInstance Win32_OperatingSystem` |
| `Get-HotFix` | Parches instalados | `Get-HotFix` |
| `Get-Culture` | Cultura regional actual | `Get-Culture` |
| `Get-Date` | Fecha y hora | `Get-Date` |
| `Get-TimeZone` | Zona horaria | `Get-TimeZone` |
| `Get-Uptime` | Tiempo encendido, si está disponible | `Get-Uptime` |
| `Get-Volume` | Volúmenes de almacenamiento | `Get-Volume` |
| `Get-Disk` | Discos físicos | `Get-Disk` |
| `Get-Partition` | Particiones | `Get-Partition` |
| `Get-Bios` | BIOS/firmware, si está disponible | `Get-CimInstance Win32_BIOS` |

### Nota práctica

- `Get-CimInstance` suele ser preferible a `Get-WmiObject` en entornos modernos.
- Para inventario remoto, `CIM` es normalmente más flexible y actual.

## 8. Archivos y hash

| Comando | Qué hace | Ejemplo |
|---|---|---|
| `Get-FileHash` | Calcula hash | `Get-FileHash .\imagen.iso -Algorithm SHA256` |
| `Get-ItemProperty` | Lee propiedades de un archivo o clave | `Get-ItemProperty .archivo.txt` |
| `Get-ChildItem -File` | Lista solo archivos | `Get-ChildItem -File` |
| `Get-ChildItem -Directory` | Lista solo carpetas | `Get-ChildItem -Directory` |
| `Get-ACL` | Muestra ACL/permisos | `Get-ACL .archivo.txt` |
| `Set-ACL` | Aplica ACL/permisos | `Set-ACL .archivo.txt $acl` |

## 9. Compresión y archivado

| Comando | Qué hace | Ejemplo |
|---|---|---|
| `Compress-Archive` | Crea ZIP | `Compress-Archive .\carpeta .backup.zip` |
| `Expand-Archive` | Extrae ZIP | `Expand-Archive .backup.zip .\destino` |

### Ejemplos prácticos

```powershell
Compress-Archive -Path .\proyecto\* -DestinationPath .\proyecto.zip
Expand-Archive -Path .\proyecto.zip -DestinationPath .
estaurado
```

## 10. Variables y shell

| Comando | Qué hace | Ejemplo |
|---|---|---|
| `$env:VAR` | Variable de entorno | `$env:Path` |
| `$PSVersionTable` | Versión de PowerShell | `$PSVersionTable` |
| `Set-Variable` | Crea o modifica variable | `Set-Variable -Name x -Value 10` |
| `Get-Variable` | Lista variables | `Get-Variable x` |
| `New-Alias` | Crea alias | `New-Alias ll Get-ChildItem` |
| `Set-Alias` | Define alias | `Set-Alias gs Get-Service` |
| `Remove-Variable` | Elimina variable | `Remove-Variable x` |
| `Clear-Host` | Limpia pantalla | `Clear-Host` |
| `Get-History` | Historial de comandos | `Get-History` |
| `Invoke-History` | Repite comando del historial | `Invoke-History 3` |

## 11. Scripts y control

| Comando | Qué hace | Ejemplo |
|---|---|---|
| `If / ElseIf / Else` | Control condicional | `if ($x -gt 5) { ... }` |
| `ForEach-Object` | Itera flujo de objetos | `Get-ChildItem \| ForEach-Object { $_.Name }` |
| `For` | Bucle clásico | `for ($i=0; $i -lt 10; $i++) {}` |
| `While` | Bucle condicional | `while ($true) {}` |
| `Switch` | Selección por casos | `switch ($x) { 1 { ... } }` |
| `Function` | Define función | `function Test-Thing { param($Name) ... }` |
| `Param` | Declara parámetros de script | `param([string]$Name)` |
| `Try/Catch/Finally` | Manejo de errores | `try { ... } catch { ... }` |
| `Start-Job` | Ejecuta en segundo plano | `Start-Job { Get-Process }` |
| `Receive-Job` | Recoge salida del job | `Receive-Job -Id 1` |
| `Stop-Job` | Detiene job | `Stop-Job -Id 1` |

### Plantilla mínima

```powershell
param(
    [string]$Name = 'World'
)

function Main {
    Write-Output "Hello, $Name"
}

Main
```

### Buenas prácticas

- Usar nombres claros para funciones y parámetros.
- Preferir `Get-Command`, `Get-Help` y `Get-Member` durante el aprendizaje.
- Evitar parsear texto cuando se puede trabajar con objetos.
- Usar `try/catch` para scripts robustos.

## 12. Automatización remota

| Comando | Qué hace | Ejemplo |
|---|---|---|
| `Invoke-Command` | Ejecuta comandos remotos | `Invoke-Command -ComputerName srv1 -ScriptBlock { Get-Service }` |
| `Enter-PSSession` | Abre sesión interactiva remota | `Enter-PSSession -ComputerName srv1` |
| `New-PSSession` | Crea sesión persistente | `New-PSSession -ComputerName srv1` |
| `Copy-Item -ToSession` | Copia a sesión remota | `Copy-Item .a.txt -ToSession $s -Destination C:\Temp` |
| `Remove-PSSession` | Cierra sesión remota | `Remove-PSSession $s` |

## 13. Seguridad y permisos

| Comando | Qué hace | Ejemplo |
|---|---|---|
| `Get-ExecutionPolicy` | Política de ejecución | `Get-ExecutionPolicy` |
| `Set-ExecutionPolicy` | Cambia política | `Set-ExecutionPolicy RemoteSigned` |
| `Get-LocalUser` | Usuarios locales | `Get-LocalUser` |
| `Get-LocalGroup` | Grupos locales | `Get-LocalGroup` |
| `Get-LocalGroupMember` | Miembros de grupo | `Get-LocalGroupMember Administrators` |
| `Add-LocalGroupMember` | Añade usuario a grupo | `Add-LocalGroupMember -Group Administrators -Member user` |
| `Get-NetFirewallRule` | Reglas de firewall | `Get-NetFirewallRule \| Select-Object DisplayName, Enabled` |
| `Enable-NetFirewallRule` | Activa regla | `Enable-NetFirewallRule -DisplayGroup 'File and Printer Sharing'` |
| `New-NetFirewallRule` | Crea regla | `New-NetFirewallRule -DisplayName 'Allow HTTPS' -Direction Inbound -Protocol TCP -LocalPort 443 -Action Allow` |

## 14. Paquetes y módulos

| Comando | Qué hace | Ejemplo |
|---|---|---|
| `Get-Module` | Lista módulos cargados o disponibles | `Get-Module -ListAvailable` |
| `Import-Module` | Importa un módulo | `Import-Module NetTCPIP` |
| `Find-Module` | Busca módulos en repositorios | `Find-Module Pester` |
| `Install-Module` | Instala módulo | `Install-Module Pester -Scope CurrentUser` |
| `Update-Module` | Actualiza módulo | `Update-Module Pester` |
| `Uninstall-Module` | Desinstala módulo | `Uninstall-Module Pester` |

## 15. Formato y exportación

| Comando | Qué hace | Ejemplo |
|---|---|---|
| `Format-Table` | Formatea como tabla | `Get-Process \| Format-Table Name, CPU` |
| `Format-List` | Formato de lista | `Get-Service \| Format-List *` |
| `Export-Csv` | Exporta a CSV | `Get-Process \| Export-Csv .\procesos.csv -NoTypeInformation` |
| `Import-Csv` | Importa CSV | `Import-Csv .\procesos.csv` |
| `ConvertTo-Json` | Convierte a JSON | `Get-Process \| Select-Object -First 1 \| ConvertTo-Json` |
| `ConvertFrom-Json` | Lee JSON | `Get-Content .\data.json \| ConvertFrom-Json` |

## 16. Acciones frecuentes rápidas

### Ver procesos top por CPU

```powershell
Get-Process | Sort-Object CPU -Descending | Select-Object -First 10
```

### Ver IPs del equipo

```powershell
Get-NetIPAddress | Where-Object AddressFamily -eq IPv4
```

### Buscar errores en logs

```powershell
Select-String -Path .\*.log -Pattern 'ERROR|FATAL'
```

### Reiniciar un servicio

```powershell
Restart-Service Spooler
```

### Crear ZIP de una carpeta

```powershell
Compress-Archive -Path .\proyecto\* -DestinationPath .\proyecto.zip
```

### Obtener hash de un archivo

```powershell
Get-FileHash .\imagen.iso -Algorithm SHA256
```

## 17. Comandos que conviene memorizar primero

```powershell
Get-Help Get-ChildItem
Get-Command *service*
Get-Location
Set-Location C:\Windows
Get-ChildItem -Force
New-Item -ItemType Directory -Path .\logs
Copy-Item .a.txt D:\BackupRemove-Item .	emp -Recurse -Force
Get-Content .app.log -Tail 20
Select-String -Path .\*.log -Pattern 'error'
Get-Process | Sort-Object CPU -Descending | Select-Object -First 5
Get-Service
Restart-Service Spooler
Test-NetConnection localhost -Port 443
Get-FileHash .archivo.iso -Algorithm SHA256
```
