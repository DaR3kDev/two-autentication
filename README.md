
# Two Authentication — Django Two-Factor Auth

Proyecto basado en **Django** con integración de **autenticación en dos pasos (2FA)**, totalmente configurado para ejecutarse con **Docker** y **Jenkins CI/CD**.

Este proyecto implementa la librería [django-two-factor-auth](https://github.com/jazzband/django-two-factor-auth) dentro de un entorno dockerizado, con un pipeline de despliegue automatizado mediante Jenkins.

---
## Características principales

* Autenticación 2FA con `django-two-factor-auth`
* Despliegue completo con `Docker` y `docker-compose`
* Pipeline automatizado con `Jenkins`
* Base de datos SQLite por defecto

---

## Estructura del proyecto

```bash
two-autentication/
├── django-two-factor-auth/    # Librería integrada
├── example/                   # Proyecto Django base de demostración
├── two_factor/                # App principal de autenticación
├── docker-compose.yml         # Orquestación de contenedores
├── Dockerfile.python          # Imagen del entorno Django
├── Dockerfile.jenkins         # Imagen del servidor Jenkins
├── Jenkinsfile                # Pipeline declarativo
├── pyproject.toml             # Dependencias y configuración del proyecto
├── Makefile
└── README.md
```

---

## Requisitos previos

Antes de iniciar asegúrate de tener instalados:

* [Docker](https://www.docker.com/get-started)
* [Docker Compose](https://docs.docker.com/compose/)
* [Jenkins](https://www.jenkins.io/) (si vas a usar el pipeline)
* [Git](https://git-scm.com/)

---

## Ejecución con Docker Compose

Para iniciar todos los servicios (Django + Jenkins), ejecuta:

```bash
docker-compose up -d
```

Esto levantará los siguientes contenedores:

| Servicio  | Puerto | Descripción             |
| --------- | ------ | ----------------------- |
| `django`  | 8000   | Servidor Django con 2FA |
| `jenkins` | 8080   | Interfaz web de Jenkins |

---

### Acceso a los servicios

* **Django:** [http://localhost:8000](http://localhost:8000)
* **Jenkins:** [http://localhost:8080](http://localhost:8080)

---

## Capturas del entorno

<p align="center">
  <img width="900" src="https://github.com/user-attachments/assets/a3a728b9-5ebb-4392-9515-69f04777cd79" alt="Docker Compose ejecutándose">
</p>

<p align="center">
  <img width="900" src="https://github.com/user-attachments/assets/570511ba-481d-46c2-b826-87d0ff3ea386" alt="Jenkins Dashboard">
</p>

<p align="center">
  <img width="900" src="https://github.com/user-attachments/assets/123bd7d3-c909-4ee1-848f-f67ba25ee2c5" alt="Django Running">
</p>

<p align="center">
  <img width="900" src="https://github.com/user-attachments/assets/486c40bb-c2f5-4b7d-bac1-1439a3aa1c1a" alt="Jenkins Building">
</p>

---

## Token de acceso a DockerHub

```bash
dckr_pat_Ll_xRX7_dDo2S1BSF7KCGjv6T6k
```

> **Importante:** Configura este token como una credencial secreta en Jenkins con el ID `docker-hub`.

---

##  Ejecución manual del contenedor

Si deseas correr solo el servicio de Django:

```bash
docker run -it --rm -p 8000:8000 dark093/two-autentication:latest
```

Luego abre tu navegador en:
[http://localhost:8000](http://localhost:8000)

**DockerHub:** [dark093/two-autentication](https://hub.docker.com/repository/docker/dark093/two-autentication/general)

<p align="center">
  <img width="900" src="https://github.com/user-attachments/assets/28ac635c-c70f-4f87-9758-0bb688e9ccc1" alt="DockerHub Link">
</p>

---

##  Pipeline con Jenkins

El archivo `Jenkinsfile` define el pipeline CI/CD completo.

### Etapas del pipeline

1. **Checkout:** Clona el repositorio desde GitHub
2. **Build Docker Image:** Construye la imagen con `Dockerfile.python`
3. **Login to DockerHub:** Usa credenciales almacenadas en Jenkins (`docker-hub`)
4. **Push to DockerHub:** Publica la imagen con tags (`latest` y versión)
5. **Cleanup:** Limpieza de caché y recursos al finalizar
---

## Licencia

Proyecto bajo licencia **MIT**
Libre para uso, modificación y distribución con atribución.
