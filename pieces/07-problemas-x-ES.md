### Guía de Problemas (ES)

# Guía de resolución de problemas — API de X


Esta guía recopila los problemas más comunes al integrar con la API de X, junto con sus causas probables y los pasos recomendados para diagnosticarlos y resolverlos.

Está pensada para equipos técnicos que ya tienen una integración en marcha y necesitan identificar fallos de forma rápida y sistemática.

---

### Antes de empezar

Antes de analizar un error específico, verificá lo siguiente:

* Estás utilizando la **URL base correcta** para el entorno (producción o sandbox).
* Las credenciales de autenticación están vigentes.
* El request que estás enviando coincide con la **versión de la API** documentada.

Muchos errores se originan en desalineaciones básicas entre entorno, credenciales y versión.

---

### Errores de autenticación (401 / 403)

#### Síntoma

La API responde con un error `401 Unauthorized` o `403 Forbidden`.

#### Causas comunes

* Token expirado o inválido.
* Uso de credenciales de un entorno distinto.
* Permisos insuficientes para el recurso solicitado.

#### Pasos recomendados

1. Verificá que el token esté activo y no haya sido revocado.
2. Confirmá que el token corresponda al entorno correcto.
3. Revisá los scopes o permisos asociados a la credencial.
4. Reintentá la solicitud con un token recién generado.

---

### Errores de validación (400 Bad Request)

#### Síntoma

La API rechaza la solicitud con un error `400`.

#### Causas comunes

* Campos obligatorios faltantes.
* Tipos de datos incorrectos.
* Valores fuera de rango.

#### Pasos recomendados

1. Validá el payload contra el esquema documentado.
2. Revisá nombres de campos y estructuras anidadas.
3. Confirmá formatos esperados (fechas, identificadores, enumeraciones).

Errores de validación suelen ser determinísticos: una vez corregido el request, el problema desaparece.

---

### Respuestas inesperadas o incompletas

#### Síntoma

La API responde correctamente, pero el contenido no coincide con lo esperado.

#### Causas comunes

* Uso de filtros incorrectos.
* Cambios en el estado del recurso.
* Suposiciones implícitas sobre valores por defecto.

#### Pasos recomendados

1. Revisá los parámetros de consulta enviados.
2. Confirmá el estado actual del recurso en cuestión.
3. Consultá la documentación sobre valores opcionales y defaults.

Evitar suposiciones reduce este tipo de errores de forma significativa.

---

### Problemas con webhooks

#### Síntoma

Los eventos no llegan o llegan de forma intermitente.

#### Causas comunes

* Endpoint no disponible o lento.
* Errores 5xx en el receptor.
* Fallos en la validación de firmas.

#### Pasos recomendados

1. Verificá que el endpoint responda rápidamente con `200 OK`.
2. Revisá logs de delivery y reintentos.
3. Confirmá la implementación de validación de firmas.
4. Asegurate de manejar eventos duplicados.

Recordá que los webhooks tienen garantía *at-least-once*.

---

### Timeouts y errores 5xx

#### Síntoma

La API responde con errores `5xx` o timeouts.

#### Causas comunes

* Picos de tráfico.
* Requests complejos o no optimizados.
* Problemas temporales de red.

#### Pasos recomendados

1. Implementá reintentos con backoff exponencial.
2. Evitá reintentar inmediatamente de forma agresiva.
3. Registrá el error y reintentá más tarde.

Los errores 5xx suelen ser transitorios. El cliente debe estar preparado para eso.

---

### Buenas prácticas para diagnóstico

* Loguear requests y responses relevantes.
* Incluir identificadores de correlación cuando sea posible.
* Probar los mismos requests en un entorno controlado.
* Aislar variables: cambiar una cosa por vez.

La observabilidad es parte de la integración, no un agregado posterior.

---

### Cuándo contactar soporte

Contactá al equipo de soporte si:

* El error persiste luego de seguir esta guía.
* Observás comportamientos inconsistentes entre entornos.
* Recibís errores no documentados.

Incluí siempre:

* endpoint afectado
* request ID (si aplica)
* timestamp
* payload resumido

Eso acelera mucho la resolución.


