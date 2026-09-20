# windows-ad-wazuh-soc-lab


# Enterprise Identity Defense: Active Directory & SIEM Telemetry with Wazuh

## 📌 Resumen Ejecutivo
Implementación de un entorno de detección defensivo (Blue Team) para auditar eventos de autenticación e identidad corporativa. El laboratorio simula un entorno empresarial con Active Directory Domain Services (AD DS) bajo Windows Server 2022, integrado con Wazuh SIEM/XDR sobre Docker para ingesta de telemetría y detección de anomalías de acceso.

---

## 🏗️ Arquitectura de Red
- **Endpoint Víctima / DC:** Windows Server 2022 (IP: `192.168.18.217` / Dominio: `labdefensivo.local` / Hostname: `DC01`)
- **SIEM / Telemetría:** Wazuh SIEM v4.9.0 (Manager, Indexer, Dashboard) sobre Docker en Linux Host (`192.168.10.50`)
- **Canal de Transporte:** Canal cifrado agente-servidor (TCP 1514)

[ Windows Server 2022 (DC01) ]


│ (Wazuh Agent / TCP 1514)
▼

[ Host Linux - Docker Stack ] ──► [ Wazuh Manager ] ──► [ OpenSearch Indexer ] ──► [ Dashboard UI ]


---

## ⚙️ Despliegue Técnico
1. **Controlador de Dominio:** Promoción de Windows Server a Domain Controller creando el bosque raíz `labdefensivo.local` y estructuración de Unidades Organizativas (`SOC_Lab`).
2. **Optimización de Recursos SIEM:** Despliegue de stack Wazuh en Docker Compose con límites estrictos de memoria JVM (`-Xms512m -Xmx512m`) y rotación de logs a nivel de motor de contenedores (`max-size: 50m`).
3. **Pipeline de Logs:** Despliegue del agente Wazuh (`WazuhSvc`) en Windows Server para el streaming continuo del canal `Security` del Visor de Eventos.

---

## 🔍 Simulación y Detección de Amenazas
Se ejecutaron simulaciones de intentos de autenticación fallidos consecutivos (fuerza bruta / validación de credenciales inválidas) contra la cuenta `testuser`.

- **Telemetría Endpoint (Event Viewer):** Captura de múltiples eventos con severidad **Audit Failure** bajo el **Event ID 4625** (Logon Type 2 / Interactivo).
- **Ingesta y Correlación SIEM:** Ingesta inmediata en Wazuh Threat Hunting, disparando alertas catalogadas con la regla correspondiente a fallos de inicio de sesión en Windows.

---
