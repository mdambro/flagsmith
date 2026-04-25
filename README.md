# 🚩 Flagsmith Self-Hosted

[![Docker](https://img.shields.io/badge/docker-%232496ED.svg?style=flat&logo=docker&logoColor=white)](https://www.docker.com/)
[![Flagsmith](https://img.shields.io/badge/flagsmith-%23E01E5A.svg?style=flat)](https://www.flagsmith.com/)

Bienvenido a la implementación autogestionada de **Flagsmith**. Esta configuración es ideal para **proyectos personales** y de desarrollo, permitiéndote tener control total sobre tus Feature Flags y configuraciones remotas sin las limitaciones de peticiones o asientos de las capas gratuitas en la nube.

## 🚀 Características Principales

- **Gestión Total:** Sin límites de usuarios, proyectos o entornos.
- **Privacidad:** Tus datos permanecen en tu propia infraestructura.
- **Feature Toggling:** Controla el despliegue de funcionalidades en tiempo real.
- **Segmentación:** Personaliza la experiencia del usuario basada en rasgos y comportamientos.

## 🛠️ Requisitos Previos

- [Docker](https://docs.docker.com/get-docker/) y [Docker Compose](https://docs.docker.com/compose/install/) instalados.

## 📦 Instalación y Despliegue

1. **Configurar el entorno:**
   Asegúrate de tener un archivo `.env` en el directorio `docker-compose` con las variables necesarias (ver sección de [Configuración](#-configuración)).

2. **Levantar los servicios:**
   ```bash
   cd docker-compose
   docker-compose up -d
   ```

3. **Verificar estado:**
   Una vez ejecutado, el dashboard debería estar accesible en `http://localhost:8000`.

## 🔑 Configuración Inicial (Superusuario)

Para poder acceder al panel de administración por primera vez, necesitas crear una cuenta de superusuario:

```bash
cd docker-compose
docker-compose run --rm flagsmith createsuperuser
```
*Sigue las instrucciones en la terminal para configurar tu email y contraseña.*

## 🏁 Pasos Post-Instalación

Una vez que hayas creado tu superusuario, sigue estos pasos para empezar a usar Flagsmith:

1. **Login:** Entra en `http://localhost:8000` con tus credenciales.
2. **Crear Organización:** Dale un nombre a tu organización personal.
3. **Crear Proyecto:** Define tu primer proyecto (ej. "Mi App Increíble").
4. **Obtener API Key:** Ve a la configuración del entorno (Environment Settings) y copia la `Server Side SDK Key` o la `Client Side ID` según lo necesites.

## ⚙️ Configuración (.env)

Para que el proyecto funcione correctamente, asegúrate de tener un archivo `.env` en el directorio `docker-compose` con el siguiente contenido base:

```bash
# All environments variables are available here:
# API: https://docs.flagsmith.com/deployment/locally-api#environment-variables
# UI: https://docs.flagsmith.com/deployment/locally-frontend#environment-variables

POSTGRES_DB=flagsmith
POSTGRES_PASSWORD=SECURE_PASSWORD
DATABASE_URL=postgresql://postgres:${POSTGRES_PASSWORD}@postgres:5432/flagsmith
# Store API and Flag Analytics data in Postgres
USE_POSTGRES_FOR_ANALYTICS=true
ENVIRONMENT=production
DJANGO_ALLOWED_HOSTS=*
# For productions environment, replace with domain address
FLAGSMITH_DOMAIN=localhost:8000
DJANGO_SECRET_KEY=secretkey
# Uncomment to prevent any additional signups
# PREVENT_SIGNUP=true
# Uncomment and set to false to only allow signups via invitations
#ALLOW_REGISTRATION_WITHOUT_INVITE=true

# Enable Task Processor
TASK_RUN_METHOD=TASK_PROCESSOR # other options are: SYNCHRONOUSLY, SEPARATE_THREAD (default)
PROMETHEUS_ENABLED=true

# Uncomment if you want to enable Google OAuth. Note this does not turn Google OAuth on. You still need to use
# Flagsmith on Flagsmith to enable it - https://docs.flagsmith.com/deployment/#oauth_google
# DJANGO_SECURE_CROSS_ORIGIN_OPENER_POLICY='same-origin-allow-popups'

# For more info on configuring E-Mails - https://docs.flagsmith.com/deployment/locally-api#environment-variables
# Example SMTP:
# EMAIL_BACKEND=django.core.mail.backends.smtp.EmailBackend
# EMAIL_HOST=mail.example.com
# SENDER_EMAIL=flagsmith@example.com
# EMAIL_HOST_USER=flagsmith@example.com
# EMAIL_HOST_PASSWORD=smtp_account_password
# EMAIL_PORT=587 # optional
# EMAIL_USE_TLS='true' # optional

```

### Detalle de Variables

| Variable | Importancia |
|----------|-------------|
| `DATABASE_URL` | Conexión a la base de datos PostgreSQL. |
| `DJANGO_SECRET_KEY` | Clave para cifrado y sesiones. ¡Mantenla secreta! |
| `DJANGO_ALLOWED_HOSTS` | Hosts permitidos (usa `*` para local o tu dominio específico). |
| `FLAGSMITH_DOMAIN` | Dominio donde corre la instancia. |

## 🧪 Ejemplo de Integración

### JavaScript SDK
```javascript
import flagsmith from 'flagsmith';

flagsmith.init({
    environmentID: "TU_CLIENT_SIDE_ID",
    api: "http://localhost:8000/api/v1/",
    onChange: (oldFlags, params) => {
        if (flagsmith.hasFeature("nueva_funcionalidad")) {
            // Tu lógica aquí
        }
    }
});
```

---
Este repositorio está optimizado para facilitar el despliegue rápido. ¡Disfruta de la libertad de gestionar tus flags a tu manera! 🚀
