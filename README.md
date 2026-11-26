# Proyecto 4 — Auditor de Red y Mapeo de Topología (Redes II)

## Estado actual
- Backend FastAPI implementado.
- Escaneo Nmap con perfiles y argumentos personalizados.
- Topología y traceroute con Scapy.
- Auditoría de puertos del router Wi-Fi.
- Lista blanca persistente por dirección MAC.
- Historial y persistencia en SQLite.
- Reportes JSON, CSV y PDF.
- Dashboard web HTML/CSS/JavaScript integrado.
- Topología central con nodos por IP, tipo de dispositivo y sistema operativo detectado.

## Requisitos previos
- Python 3.12+ (recomendado 3.12 o 3.13)
- Nmap 7.9+ con Npcap instalado y agregado al PATH
- Ejecutar la terminal/VS Code como **Administrador**

## Cómo levantar el sistema
```powershell
cd backend
py -m venv venv
.\venv\Scripts\python.exe -m pip install --upgrade pip
.\venv\Scripts\python.exe -m pip install -r requirements.txt
.\venv\Scripts\python.exe -m uvicorn main:app --host 127.0.0.1 --port 8000 --reload
```

Abrir `http://127.0.0.1:8000/` en el navegador. El dashboard web se sirve desde la carpeta `frontend/`.

## QA y alcance actual

### Verificado en esta revisión
- Sintaxis de los módulos Python del backend (`python -m compileall`).
- Sintaxis de `frontend/app.js` (`node --check`).
- Manejadores de historial, Nmap, Scapy ARP, auditoría de router y traceroute con respuestas simuladas, sin enviar tráfico a la red.
- Interfaz del navegador con respuestas simuladas: descubrimiento Scapy, carga desde historial, resúmenes de router/traceroute y agregar/eliminar en lista blanca.
- Generación de JSON, CSV y PDF con datos de prueba para escaneos Nmap, Scapy y auditorías de router.

### Implementado, pero pendiente de prueba en una red de laboratorio autorizada
- Ejecución real de perfiles Nmap y argumentos personalizados; auditoría del router y traceroute.
- Descubrimiento ARP real con Scapy, gestión de la lista blanca, historial SQLite y descarga desde el navegador.
- En este entorno Nmap no está disponible en `PATH` y Scapy informa que no hay proveedor libpcap. Para validar esos flujos instala Nmap y Npcap, abre VS Code como administrador y prueba solo en una red propia o autorizada.

### Ampliaciones futuras (no implementadas)
- Acceso remoto por Paramiko/SSH y backups.
- Sockets TCP en una VM Debian.
- Captura BPF continua.
- VPN Mesh con Tailscale o ZeroTier.

## API principal
- `GET /api/status`: estado y configuración de red.
- `POST /api/scan`: escaneo Nmap.
- `POST /api/audit-router`: auditoría del router.
- `POST /api/topology/scapy`: descubrimiento ARP.
- `GET /api/topology/traceroute/{target_ip}`: ruta ICMP.
- `GET /api/whitelist` y `POST /api/whitelist`: consulta y registro.
- `DELETE /api/whitelist/{mac}`: eliminación.
- `GET /api/history`: historial.
- `GET /api/history/{scan_id}`: detalle.
- `GET /api/export/{scan_id}/{json|csv|pdf}`: descarga de reportes.