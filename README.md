# 🤖 Agente de Consultas de CV (n8n + Qdrant + ElevenLabs)

Este proyecto implementa un sistema de agentes inteligentes para analizar y consultar información de Curricula Vitae (CV) almacenados en Google Drive, utilizando flujos de n8n.

---

## 🛠️ Requisitos del Sistema

Para ejecutar este proyecto, necesitas lo siguiente:

* **Docker y Docker Compose:** Para levantar n8n y Qdrant.
* **Servicios con API Key:** Qdrant (Base de Datos Vectorial) y ElevenLabs (Texto-a-Voz).
* **Google Drive:** Para el almacenamiento de los archivos PDF de los CVs.
* **Cloudflare Tunnel (Cloudflared):** Para exponer el webhook del agente conversacional de voz.

---

## ⚙️ Configuración del Entorno

### 1. Preparación de Archivos

1.  Asegúrate de que tus flujos de n8n (`.json`) estén guardados en la carpeta `workflows/`.
2.  Crea tu archivo `.gitignore` e incluye al menos la línea:
    ```
    .env
    ```
3.  Copia el archivo `.env.example` y renómbralo a **`.env`** (Este archivo contendrá tus secretos y NO debe subirse a Git).
    ```bash
    cp .env.example .env
    ```

### 2. Variables de Entorno (`.env`)

Edita el archivo **`.env`** local (el que no subes) y rellena todos los valores sensibles y de configuración:

| Variable | Descripción |
| :--- | :--- |
| `N8N_WEBHOOK_URL` | URL base de tu instalación n8n (o el dominio Cloudflared) |
| `N8N_USER`, `N8N_PASSWORD` | Credenciales de acceso de tu instancia n8n (si usas autenticación básica). |
| `QDRANT__SERVICE__API_KEY` | Tu clave API de Qdrant. |
| `ELEVENLABS_API_KEY` | Tu clave API de ElevenLabs. |

### 3. Inicialización con Docker

Se recomienda utilizar Docker Desktop, para facilitar la instalación de las herramientas.





🚀 Flujos de Trabajo (Workflows)
Debes importar los archivos .json de la carpeta workflows/ en tu instancia de n8n. IMPORTANTE: Después de importar, debes re-conectar manualmente las credenciales de Google Drive, Qdrant y ElevenLabs a los nodos correspondientes dentro de cada flujo.

1. Analisis CV (Analisis CV.json)
Función: Procesa nuevos archivos PDF (CVs) desde Google Drive y los carga en tu base de datos de Qdrant.
Uso: Se ejecuta automáticamente cuando se sube un archivo PDF (con los Curriculum Vitae) a la cuenta de Google Drive configurada.

2. AgenteChatbotCV (AgenteChatbotCV.json)
Función: Agente de consulta de CVs mediante una interfaz de chat simple, utilizando la información indexada en Qdrant.
Uso: Accede a la URL del webhook del flujo para iniciar la conversación.

3. AgenteCVvoz (AgenteCVvoz.json)
Función: Agente conversacional que maneja la consulta por voz. Utiliza ElevenLabs para la síntesis de voz.
Uso: Requiere que se configure y ejecute un túnel de Cloudflared que apunte a tu instalación de n8n para manejar las peticiones de audio/voz de forma pública.





