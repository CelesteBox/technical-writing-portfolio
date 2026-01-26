# Artículo Técnico - Python (ES)

### Crear y activar un entorno virtual en Python (dev-to-dev)

Si trabajás en Python y no usás entornos virtuales, tarde o temprano algo se rompe. No por mala praxis, sino porque el ecosistema de dependencias no perdona: versiones incompatibles, paquetes globales pisados, proyectos que dejan de correr sin razón aparente.

Un entorno virtual no es una buena práctica “nice to have”. Es aislamiento básico para poder trabajar con previsibilidad.

Este artículo asume que ya sabés qué es Python, qué es `pip` y que usás la terminal sin miedo.

### Qué problema resuelve un entorno virtual (sin romanticismo)

Un entorno virtual crea un espacio aislado donde:

* las dependencias pertenecen **a un proyecto**, no a tu sistema
* las versiones quedan controladas
* reproducir el entorno en otra máquina es posible

Nada más. Nada menos.

Si instalás todo globalmente, estás mezclando contextos. Y mezclar contextos en software casi siempre termina mal.

### Opción estándar: `venv` (lo que trae Python)

Desde Python 3.3 en adelante, `venv` viene incluido. No necesitás instalar nada extra.

Desde la raíz de tu proyecto:

```bash
python -m venv venv
```

Convención habitual: llamar al entorno `venv/` o `.venv/`. El nombre no importa; la consistencia sí.

Eso crea una carpeta con el intérprete de Python y `pip` aislados.

### Activar el entorno (acá suelen empezar los errores)

La activación depende del sistema operativo.

**macOS / Linux**

```bash
source venv/bin/activate
```

**Windows (PowerShell)**

```powershell
venv\Scripts\Activate.ps1
```

Si todo salió bien, vas a ver algo así al inicio del prompt:

```text
(venv)
```

Ese prefijo indica que cualquier `python` o `pip` que ejecutes pertenece al entorno virtual, no al sistema.

Si no aparece, no está activo. No hay estados intermedios.

### Verificación rápida (sin confiar ciegamente)

Chequeá qué Python estás usando:

```bash
which python
```

o en Windows:

```powershell
where python
```

La ruta debería apuntar al directorio del entorno virtual. Si apunta a `/usr/bin/python` o similar, algo no está bien.

### Instalar dependencias dentro del entorno

Con el entorno activo:

```bash
pip install requests
```

Ese paquete queda instalado **solo** en ese entorno. Otro proyecto no lo ve. Ese es el punto.

Para persistir dependencias:

```bash
pip freeze > requirements.txt
```

Y para reproducirlas en otra máquina:

```bash
pip install -r requirements.txt
```

Nada nuevo para un dev, pero sorprendentemente fácil de olvidar.

### Errores comunes (y por qué pasan)

* **“Instalé el paquete pero Python no lo encuentra”**
  → Instalaste con el entorno desactivado.

* **“Tengo dos versiones distintas del mismo paquete”**
  → Mezcla de global + entorno virtual.

* **“En mi máquina funciona”**
  → No versionaste dependencias.

Los entornos virtuales no eliminan errores, pero hacen que los errores sean rastreables. Eso ya es una mejora enorme.

### Cuándo esto no alcanza

Para proyectos más complejos (múltiples versiones de Python, tooling más pesado), entran en juego `pyenv`, `poetry`, `pipenv`, contenedores, etc.

Pero incluso en esos casos, el concepto base sigue siendo el mismo: **aislar contextos**.

Si dominás esto, lo demás es tooling.

---

👉 Pegalo en su `.md`, hacé **una sola pasada de edición**, y cuando lo cierres seguimos con el siguiente del día.
