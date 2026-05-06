# 🤖 AITalk - Sistema Inteligente de Atención al Cliente n8n

**AITalk** es un sistema automatizado e inteligente de atención al cliente diseñado para el sector de la hostelería (cadenas de restaurantes). Basado en una arquitectura de IA agéntica, este proyecto implementa un asistente virtual capaz de gestionar reservas, responder consultas sobre el menú y alérgenos, y mantener una conversación fluida en lenguaje natural a través de Telegram.

Este repositorio contiene los flujos de trabajo principales desarrollados e implementados sobre el orquestador **n8n**.

---

## 🛠️ Tecnologías y Arquitectura

El core de la aplicación está construido mediante una arquitectura basada en nodos modulares centralizados en n8n:

*   **Orquestador:** [n8n](https://n8n.io/) (Self-hosted local).
*   **Cerebro del Agente (LLM):** Mistral AI (`devstral latest` a través de Mistral Studio) por su baja latencia, disponibilidad y alta capacidad de compresión.
*   **Base de Datos:** [PostgreSQL](https://www.postgresql.org/) (Gestión relacional del restaurante y persistencia de la memoria conversacional).
*   **Gestor de Calendario:** Google Calendar API (vía Google Cloud Console).
*   **Interfaz de Usuario:** Telegram Bot API.
*   **Voz a Texto / Texto a Voz (Opcional):** Gemini 3.1 flash-lite (STT) y ElevenLabs (TTS).

---

## ✨ Características Principales

*   **Interfaz Conversacional Natural:** Integración directa con Telegram, permitiendo interactuar tanto por mensajes de texto como por audios.
*   **Agente Inteligente Autónomo:** Uso de un nodo AI Agent de n8n configurado con herramientas (Tools) para tomar decisiones y ejecutar acciones en tiempo real sin intervención humana.
*   **Gestión de Reservas:** 
    *   Consulta de disponibilidad utilizando `get events` de Google Calendar.
    *   Creación de eventos (`create events`) evitando solapamientos.
    *   Registro sincronizado en la base de datos de reservas del restaurante.
*   **Consultas Dinámicas en BBDD:** La IA puede ejecutar lecturas en PostgreSQL para informar al usuario sobre platos, ingredientes y alérgenos.
*   **Memoria Persistente:** Historial de conversación guardado en PostgreSQL (tabla `n8n_chat_histories`) para mantener el contexto a largo plazo y dar continuidad a las interacciones.

---

## 🏗️ Estructura del Flujo de n8n

El flujo de trabajo principal de AITalk se divide en los siguientes módulos lógicos:

1.  **Trigger (Telegram):** Nodo que escucha de manera continua la llegada de nuevos mensajes (texto o voz).
2.  **Agente IA:** Diferencia entre peticiones de texto o de voz. Utiliza el modelo Mistral con un *System Prompt* robusto que define su rol como recepcionista de restaurante y lo protege contra intentos de *Jailbreak*.
3.  **Postgres Chat Memory:** Módulo de memoria permanente que almacena el contexto de las sesiones.
4.  **Tools (Herramientas del Agente):**
    *   `Google Calendar Tool`: Crear y eliminar eventos.
    *   `PostgreSQL Select`: Ejecutar queries dinámicas para consultar información (menú, disponibilidad local).
    *   `PostgreSQL Execute`: Inserción de nuevos clientes y confirmación de reservas en BBDD.
5.  **Output (Respuesta):** Formulación de la respuesta en texto natural y envío al usuario final a través del bot de Telegram.

---

## 🚀 Requisitos Previos

Para desplegar este flujo en tu propia instancia, necesitarás:

1.  Una instancia de **n8n** instalada y ejecutándose (recomendado *Self-hosted*).
2.  Una base de datos **PostgreSQL** configurada y con las tablas necesarias instaladas. *(Consulta el volcado SQL en los anexos de la memoria o en la carpeta `/database` si la has subido al repo).*
3.  Un token de bot de **Telegram** (obtenido a través de *BotFather*).
4.  Claves de la API de **Mistral AI** (Mistral Studio).
5.  Credenciales de **Google Cloud Console** (OAuth2 o Service Account) habilitadas para el uso de la API de Google Calendar.

---

## ⚙️ Instalación y Configuración

1. **Clonar el repositorio:**
   ```bash
   git clone https://github.com/tu-usuario/aitalk-n8n.git
   ```
2. **Importar el Workflow en n8n:**
   * Abre tu interfaz de n8n.
   * Ve a **Workflows** -> **Import from File**.
   * Selecciona el archivo `.json` del workflow incluido en este repositorio.
3. **Configurar Credenciales:**
   Dentro del workflow importado, deberás crear y asignar tus credenciales a los siguientes nodos:
   * **Telegram Account:** Introduce el Token de tu bot.
   * **Mistral API:** Introduce tu clave de acceso.
   * **PostgreSQL:** Configura el host, puerto, usuario, contraseña y base de datos.
   * **Google Calendar:** Conecta tu cuenta mediante OAuth2.
4. **Activar el Trigger:**
   Cambia el interruptor superior derecho de n8n a **Active** para que el bot empiece a escuchar a través de Telegram.

---

## 👨‍💻 Autores

Proyecto de desarrollo para el **CFGS Administració de Sistemes Informàtics i Xarxes (ASIX2)** - Curso 2025-2026.
*Institut Puig Castellar - Santa Coloma de Gramenet*

*   **Pau Laguna**
*   **Joel Ortega**
*   **Liming Xin**
