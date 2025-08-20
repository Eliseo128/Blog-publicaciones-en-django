Perfecto 🙌 Te preparo una **Guía paso a paso** bien estructurada para que tus estudiantes de preparatoria (nivel principiante) puedan crear un **proyecto básico en Django** en Windows.
La guía es detallada, incluye **explicaciones**, **comandos** y **códigos comentados**.

---

# 📘 Guía Paso a Paso: Proyecto Básico en Django

---

## 🔹 1. Estructura Final Esperada

Al terminar el proyecto, la carpeta se verá así:

```
Guia_Django/
│
├── backend_blog/
│   ├── backend_blog/
│   │   ├── __init__.py
│   │   ├── asgi.py
│   │   ├── settings.py
│   │   ├── urls.py
│   │   ├── wsgi.py
│   │
│   ├── app_blog/
│   │   ├── migrations/
│   │   ├── __init__.py
│   │   ├── admin.py
│   │   ├── apps.py
│   │   ├── models.py
│   │   ├── urls.py
│   │   ├── views.py
│   │   ├── templates/
│   │   │   └── app_blog/
│   │   │       └── index.html
│   │
│   ├── manage.py
│
├── .venv/
├── requirements.txt
```

---

## 🔹 2. Procedimientos Básicos en Windows

### 2.1 Abrir la consola (cmd) en Windows

1. Presiona `Windows + R`.
2. Escribe:

   ```
   cmd
   ```
3. Presiona **Enter**.

---

### 2.2 Crear la carpeta de trabajo

En la consola escribe:

```bash
mkdir Guia_Django
cd Guia_Django
```

---

### 2.3 Abrir VS Code desde la terminal

```bash
code .
```

> Esto abrirá la carpeta actual en **Visual Studio Code**.

---

### 2.4 Abrir terminal en VS Code

En VS Code:

* Menú → **Ver → Terminal**
* O usa el atajo: `Ctrl + ñ`

---

### 2.5 Ver versión de Python

```bash
python --version
```

---

## 🔹 3. Entorno Virtual y Django

### 3.1 Crear entorno virtual (.venv)

```bash
python -m venv .venv
```

---

### 3.2 Activar entorno virtual

```bash
.venv\Scripts\activate
```

> Verás `(.venv)` al inicio de la línea.

---

### 3.3 Instalar Django

```bash
pip install django
```

---

### 3.4 Ver herramientas disponibles de Django

```bash
django-admin
```

---

## 🔹 4. Crear el Proyecto

### 4.1 Crear proyecto sin duplicar carpeta

```bash
django-admin startproject backend_blog .
```

> El punto (`.`) evita que se duplique la carpeta.

---

### 4.2 Ver estructura creada

En `backend_blog/` ahora tienes:

* manage.py
* backend\_blog/ (configuración del proyecto)

---

### 4.3 Ejecutar servidor Django

```bash
python manage.py runserver
```

---

### 4.4 Ver página web de bienvenida

* Abre en el navegador:
  👉 [http://127.0.0.1:8000/](http://127.0.0.1:8000/)

---

### 4.5 Detener servidor

Presiona `CTRL + C` en la terminal.

---

## 🔹 5. Crear la Aplicación

### 5.1 Crear app “app\_blog”

```bash
python manage.py startapp app_blog
```

---

### 5.2 Agregar app en `settings.py`

En `backend_blog/settings.py`, busca `INSTALLED_APPS` y agrega:

```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'app_blog',  # 👈 Nuestra aplicación
]
```

---

## 🔹 6. Primera Vista y URLs

### 6.1 Vista en `views.py` de app\_blog

```python
from django.http import HttpResponse

def inicio(request):
    return HttpResponse("¡Bienvenido al Blog!")
```

---

### 6.2 Crear archivo `urls.py` en app\_blog

```python
from django.urls import path
from . import views

urlpatterns = [
    path('', views.inicio, name='inicio'),
]
```

---

### 6.3 Incluir `app_blog.urls` en `backend_blog/urls.py`

```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('app_blog.urls')),  # 👈 Incluimos rutas de app_blog
]
```

---

## 🔹 7. Modelo Publicación

### 7.1 Crear modelo en `models.py`

```python
from django.db import models

class Publicacion(models.Model):
    titulo = models.CharField(max_length=200)
    contenido = models.TextField()
    fecha_de_creacion = models.DateTimeField(auto_now_add=True)

    def __str__(self):
        return self.titulo  # 👈 Muestra el título en el admin
```

---

### 7.2 Realizar migraciones

```bash
python manage.py makemigrations
```

---

### 7.3 Aplicar migraciones

```bash
python manage.py migrate
```

---

### 7.4 Registrar modelo en `admin.py`

```python
from django.contrib import admin
from .models import Publicacion

admin.site.register(Publicacion)
```

---

### 7.5 Crear superusuario

```bash
python manage.py createsuperuser
```

Valores:

* **Usuario**: admin
* **Correo**: [elisa@gmail.com](mailto:elisa@gmail.com)
* **Password**: admin

---

## 🔹 8. Templates y Vista Dinámica

### 8.1 Crear carpetas de templates

Dentro de `app_blog/`:

```
app_blog/
 ├── templates/
 │    └── app_blog/
 │         └── index.html
```

---

### 8.2 Código de `index.html` con comentarios

```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <title>Blog</title>
</head>
<body>
    <h1>Entradas del Blog</h1>

    <!-- Lista de publicaciones usando un for -->
    <ul>
        {% for pub in publicaciones %}
            <li>
                <strong>{{ pub.titulo }}</strong> - {{ pub.fecha_de_creacion }}
            </li>
        {% empty %}
            <li>No hay publicaciones disponibles.</li>
        {% endfor %}
    </ul>
</body>
</html>
```

---

### 8.3 Código actualizado de `views.py`

```python
from django.shortcuts import render
from .models import Publicacion

def inicio(request):
    publicaciones = Publicacion.objects.all()
    return render(request, 'app_blog/index.html', {'publicaciones': publicaciones})
```

---

## 🔹 9. Archivo de Requerimientos

Crear archivo:

```bash
pip freeze > requirements.txt
```

---

## 🔹 10. Ejecutar Proyecto Final

```bash
python manage.py runserver
```

* Entra a 👉 [http://127.0.0.1:8000/](http://127.0.0.1:8000/)
* Verás el **blog funcionando con publicaciones**.

---

✅ ¡Listo! Los estudiantes ahora tienen un **proyecto Django básico** con:

* Proyecto y aplicación creados
* Modelo Publicación
* Panel de administración
* Plantilla HTML con publicaciones

---

¿Quieres que te prepare también un **PDF didáctico con capturas e iconos** de cada paso para que tus alumnos lo usen como manual en clase?
