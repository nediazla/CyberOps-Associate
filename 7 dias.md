# Día 1 – Fundamentos, ataques base y modelos de defensa

**Objetivo:** crear cimientos sólidos y afianzar cómo piensan los atacantes.

Temas:

- amenazas, actores, motivaciones
    
- vulnerabilidades y exposición
    
- CIA+SA
    
- Cyber Kill Chain + MITRE
    
- ataques fundamentales: scanning, spoofing, floods, fragmentación
    
- Zero Trust
    
- Defensa en profundidad
    

Práctica:

- Analizar un escaneo Nmap de tu propia red
    
- Detectar scanning, SYN flood y ARP spoofing usando Wireshark o tcpdump
    

Meta del día:

- Reconocer patrones de ataque base en tráfico real.
    

---

# Día 2 – SOC, SIEM, telemetría y monitorización

**Objetivo:** entender cómo trabaja un SOC y cómo se investigan alertas.

Temas:

- SOC N1 / N2 / N3
    
- SIEM, normalización, enriquecimiento
    
- Syslog, Windows Logs, Linux Logs
    
- NetFlow/IPFIX
    
- Correlación
    
- Detección de anomalías
    

Práctica:

- Cargar logs en una instancia de ELK o Wazuh
    
- Consultas básicas: fallos de login, scanning, DNS anómalo
    
- Identificar alertas verdaderas vs falsas
    

Meta del día:

- Dominar la mecánica básica de investigar alertas.
    

---

# Día 3 – Windows y Linux desde perspectiva SOC

**Objetivo:** entender sistemas operativos y detectar técnicas MITRE dentro de ellos.

Temas:

- Estructura interna de Windows
    
- Windows logs críticos: 4624, 4625, 4672, 4688, 7045, 1102
    
- Sysmon (ID 1, 3, 10, 11)
    
- Linux logs: auth.log, syslog, cron, auditd
    
- Privilege escalation, persistence, credential dumping
    

Práctica:

- Instalar Sysmon (Windows) o auditd (Linux)
    
- Generar eventos: SSH login, sudo, powershell, procesos
    
- Identificar persistencia (tareas programadas, Run keys, cron jobs)
    

Meta del día:

- Ser capaz de leer y entender un ataque en los logs del host.
    

---

# Día 4 – Ataques a servicios + C2 + exfiltración

**Objetivo:** interpretar explotación directa a servicios y tráfico de C2.

Temas:

- HTTP: SQLi, XSS, LFI/RFI, webshells
    
- DNS tunneling
    
- SMB exploitation (EternalBlue)
    
- RDP brute force + hijacking
    
- SSH abuso y túneles
    
- C2 (HTTP, HTTPS, DNS, TLS, custom protocols)
    

Práctica:

- Montar ataque SQLi básico en un DVWA o Mutillidae
    
- Analizar con tcpdump o Zeek
    
- Detectar DNS tunneling (consultas TXT largas)
    
- Simular C2 con un script que haga beaconing constante
    

Meta del día:

- Identificar explotación de servicios y comunicación maliciosa.
    

---

# Día 5 – Endpoint Security, Malware y EDR

**Objetivo:** dominar comportamiento interno del endpoint.

Temas:

- EPP vs EDR
    
- Árbol de procesos
    
- Malware fileless
    
- Mimikatz, credential dumping
    
- Persistencia en Windows y Linux
    
- Indicators of Attack (IOAs)
    

Práctica:

- Generar un árbol malicioso simulado:  
    Word → PowerShell → curl
    
- Identificarlo en EDR o Sysmon
    
- Detectar acceso a LSASS (ID 10 de Sysmon)
    
- Comprobar persistencia con “autoruns” o systemctl
    

Meta del día:

- Identificar actividad sospechosa sin necesidad de firma antivirus.
    

---

# Día 6 – Vulnerabilidades, exploits y forense

**Objetivo:** ver cómo se explota un sistema y cómo analizarlo después.

Temas:

- CVE, CVSS
    
- scanners: OpenVAS / Nessus
    
- explotación real: SMB, Apache, DNS, SSH
    
- forense: memoria + disco
    
- análisis de malware básico
    
- reconstrucción de timeline
    

Práctica:

- Escanear con OpenVAS o Nmap NSE
    
- Generar un ataque:
    
    - brute force SSH
        
    - exploit básico en Metasploitable
        
- Analizar memoria: Volatility (pslist, netscan)
    
- Analizar disco: Autopsy (timeline, MFT)
    

Meta del día:

- Saber detectar explotación y realizar análisis inicial.
    

---

# Día 7 – Respuesta a incidentes + repaso examen

**Objetivo:** consolidar todo y prepararte para el examen 200-201.

Temas:

- NIST 800-61: detección, contención, erradicación, recuperación
    
- acciones del EDR: aislar, matar procesos, bloquear hashes
    
- playbooks de phishing, ransomware, brute force
    
- comunicación de incidentes
    
- documentación final
    

Práctica:

- Simular un incidente completo (phishing, C2, persistencia)
    
- Hacer triage → análisis → contención → reporte
    
- Resolver 50+ preguntas de examen
    
- Revisar MITRE tácticas clave:  
    Execution, Persistence, Credential Access, Lateral Movement, C2
    

Meta del día:

- Tener el flujo mental claro para responder cualquier escenario de examen o SOC real.
    
