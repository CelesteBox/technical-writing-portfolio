### Artículo Técnico - Python (ES)

# Crear y activar un entorno virtual en Python (dev-to-dev)

Si trabajas en Python sin entornos virtuales, tarde o temprano algo se romperá. No es mala praxis sino el ecosistema de dependencias que no perdona: habrá versiones incompatibles, paquetes globales pisados y proyectos que dejan de correr sin una razón aparente.

Un entorno virtual no es una buena práctica: es el aislamiento básico para trabajar con previsibilidad.

Algo importante: este artículo asume que sabés qué es Python, qué es `pip` y que usás la terminal sin miedo.

### Qué problema resuelve un entorno virtual

Crea un espacio aislado donde:
* las dependencias pertenecen **a un proyecto**, no a tu sistema,
* las versiones están controladas,
* es posible reproducir el entorno en otra máquina.

Si instalás todo globalmente, mezclas contextos. Y eso puede terminar mal.

### Opción estándar: `venv`

Desde Python 3.3 en adelante, `venv` viene incluido. 

Desde la raíz de tu proyecto:

```bash
python -m venv venv
```

Usualmente se llama al entorno `venv/` o `.venv/`. El nombre es opcional, la consistencia es vital.

Eso crea una carpeta con el intérprete de Python y `pip` aislados.

### Activar el entorno según el sistema operativo

**macOS / Linux**

```bash
source venv/bin/activate
```

**Windows (PowerShell)**

```powershell
venv\Scripts\Activate.ps1
```

Verás algo así al inicio del prompt:

```text
(venv)
```

Ese prefijo te indica que cualquier `python` o `pip` que ejecutes pertenece al entorno virtual. Si no aparece, el entorno no está activo. 

### Verificación rápida 

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

Ese paquete queda instalado **solo** en ese entorno, y otro proyecto no lo verá.

Para persistir dependencias:

```bash
pip freeze > requirements.txt
```

Y para reproducirlas en otra máquina:

```bash
pip install -r requirements.txt
```


### Errores comunes

* **“Instalé el paquete pero Python no lo encuentra”**
  → Instalaste con el entorno desactivado.

* **“Tengo dos versiones distintas del mismo paquete”**
  → Mezcla de global + entorno virtual.

* **“En mi máquina funciona”**
  → No versionaste dependencias.

Los entornos virtuales no eliminan errores, pero los vuelven **rastreables**. 

### Cuándo esto no alcanza

Para proyectos más complejos (múltiples versiones de Python o tooling más pesado), entran en juego `pyenv`, `poetry`, `pipenv`, contenedores, etc. Pero el concepto es el mismo: **aislar contextos**.
---
