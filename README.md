# 🤖 AITalk - Sistema Inteligente de Atención al Cliente n8n

**AITalk** es un sistema automatizado e inteligente de atención al cliente diseñado para el sector de la hostelería (cadenas de restaurantes). Basado en una arquitectura de IA agéntica, este proyecto implementa un asistente virtual capaz de gestionar reservas, responder consultas sobre el menú y alérgenos, y mantener una conversación fluida en lenguaje natural a través de Telegram.

Este repositorio contiene los flujos de trabajo principales desarrollados e implementados sobre el orquestador **n8n**.

---

## 🛠️ Tecnologías y Arquitectura

El core de la aplicación está construido mediante una arquitectura basada en nodos modulares centralizados en n8n:

*   **Orquestador:** [n8n](https://n8n.io/) (Self-hosted local).
*   **Cerebro del Agente (LLM):** Mistral AI por su baja latencia, disponibilidad y alta capacidad de compresión.
*   **Base de Datos:** [PostgreSQL](https://www.postgresql.org/).
*   **Gestor de Calendario:** Google Calendar API.
*   **Interfaz de Usuario:** Telegram Bot API.
*   **Voz a Texto / Texto a Voz (Opcional):** Gemini 3.1 flash-lite y ElevenLabs.

---

## ✨ Características Principales

*   **Interfaz Conversacional Natural:** Integración directa con Telegram, permitiendo interactuar tanto por mensajes de texto como por audios.
*   **Agente Inteligente Autónomo:** Uso de un nodo AI Agent de n8n configurado con herramientas para tomar decisiones y ejecutar acciones en tiempo real sin intervención humana.
*   **Gestión de Reservas:** 
    *   Consulta de disponibilidad utilizando `get events` de Google Calendar.
    *   Creación de eventos evitando solapamientos.
    *   Registro sincronizado en la base de datos de reservas del restaurante.
*   **Consultas Dinámicas en BBDD:** La IA puede ejecutar lecturas en PostgreSQL para informar al usuario sobre platos, ingredientes y alérgenos.
*   **Memoria Persistente:** Historial de conversación guardado en PostgreSQL para mantener el contexto a largo plazo y dar continuidad a las interacciones.

---

## 🏗️ Estructura del Flujo de n8n

El flujo de trabajo principal de AITalk se divide en los siguientes módulos lógicos:

1.  **Trigger de Telegram:** Nodo que escucha de manera continua la llegada de nuevos mensajes.
2.  **Agente IA:** Diferencia entre peticiones de texto o de voz. Utiliza el modelo Mistral con un *System Prompt* robusto que define su rol como recepcionista de restaurante y lo protege contra intentos de *Jailbreak*.
3.  **Postgres Chat Memory:** Módulo de memoria permanente que almacena el contexto de las sesiones.
4.  **Tools:**
    *   `Google Calendar Tool`: Crear y eliminar eventos.
    *   `PostgreSQL Select`: Ejecutar queries dinámicas para consultar información (menú, disponibilidad local).
    *   `PostgreSQL Execute`: Inserción de nuevos clientes y confirmación de reservas en BBDD.
5.  **Output:** Formulación de la respuesta en texto natural y envío al usuario final a través del bot de Telegram.

---

## 👨‍💻 Autores

Proyecto de desarrollo para el **CFGS Administració de Sistemes Informàtics i Xarxes (ASIX2)** - Curso 2025-2026.
*Institut Puig Castellar - Santa Coloma de Gramenet*

*   **Pau Laguna**
*   **Joel Ortega**
*   **Liming Xin**
