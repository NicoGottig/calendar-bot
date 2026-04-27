# calendar-bot

Bot en Python que consulta Google Calendar vía la API oficial y muestra los eventos del día en consola. Se configura posteriormente con Telegram para notificar al usuario. Está pensado para correr de forma automatizada (por ejemplo con GitHub Actions) sin intervención manual una vez autenticado.

## Estructura

```
calendar-bot/
├── auth_once.py          # Autenticación OAuth2 interactiva (correr una sola vez)
├── test_calendar.py      # Script principal: lista calendarios y eventos del día
├── requirements.txt      # Dependencias
├── src/                  # Código fuente adicional
└── .github/workflows/    # Automatización con GitHub Actions
```

## Cómo funciona

El flujo tiene dos pasos. Primero se corre `auth_once.py` de forma interactiva: abre el navegador, el usuario autoriza el acceso a Google Calendar (scope de solo lectura), y el token resultante se guarda en `token.json`. A partir de ahí, `test_calendar.py` carga ese token, lo refresca automáticamente si expiró, y consulta todos los calendarios accesibles para listar los eventos del día con zona horaria `America/Argentina/Buenos_Aires`.

## Requisitos previos

Se necesita un proyecto en [Google Cloud Console](https://console.cloud.google.com/) con la Calendar API habilitada y credenciales OAuth2 descargadas como `credentials.json` en la raíz del proyecto.

## Instalación

```bash
python -m venv .venv
source .venv/bin/activate  # Windows: .venv\Scripts\activate
pip install -r requirements.txt
```

## Uso

```bash
# Solo la primera vez: genera token.json
python auth_once.py

# Consultar eventos del día
python test_calendar.py
```

## Seguridad

Los archivos sensibles (`credentials.json`, `token.json`, `.env`) están excluidos del repositorio vía `.gitignore`. **Nunca commiteés esos archivos.** Si usás GitHub Actions, almacená las credenciales como [Secrets del repositorio](https://docs.github.com/en/actions/security-guides/encrypted-secrets).

## Dependencias

```
google-api-python-client
google-auth
google-auth-httplib2
google-auth-oauthlib
requests
```
