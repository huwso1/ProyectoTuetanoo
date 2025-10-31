# Documentacion API SECOP II (datos.gov.co)

## 1. Introduccion
El dataset `p6dx-8zbt` expone informacion de procesos de contratacion publicados en SECOP II. Es administrado en la plataforma Socrata de datos abiertos de Colombia y permite consultar, filtrar y agregar registros mediante parametros REST y el lenguaje SoQL (Socrata Query Language).

- **Endpoint base JSON:** `https://www.datos.gov.co/resource/p6dx-8zbt.json`
- **Formato de respuesta:** JSON (UTF-8)
- **Autenticacion:** Publica. Para uso intensivo se recomienda registrar un `app_token` gratuito en datos.gov.co y enviarlo en el encabezado `X-App-Token`.

## 2. Buenas practicas de consumo
- Limite la cantidad de registros por peticion con `$limit` (maximo 5000 sin token, 50000 con token) y pagine usando `$offset`.
- Ordene resultados con `$order` para obtener salidas deterministicas.
- Compose condiciones en `$where` usando tipos correctos (fechas ISO 8601, numeros sin separadores, textos entre comillas simples).
- Evite caracteres especiales en la URL codificandolos con percent-encoding.
- Revise periodicamente metadatos del dataset para detectar cambios de esquema.

## 3. Mapeo de campos principales
| Campo API | Nombre legible | Descripcion | Tipo Socrata |
|-----------|----------------|-------------|--------------|
| `entidad` | Entidad | Nombre de la entidad que publica el proceso | texto |
| `nit_entidad` | NIT Entidad | Nit de la entidad que publico el proceso | texto |
| `departamento_entidad` | Departamento Entidad | Departamento de la entidad | texto |
| `ciudad_entidad` | Ciudad Entidad | Ciudad de la entidad | texto |
| `ordenentidad` | Orden Entidad | Orden (Nacional, Regional) | texto |
| `codigo_pci` | Entidad Centralizada | Indica si la entidad es centralizada | texto |
| `id_del_proceso` | ID del Proceso | Identificador unico de SECOP II | texto |
| `referencia_del_proceso` | Referencia del Proceso | Identificador dado por la entidad | texto |
| `ppi` | PCI | Codigo de unidad/subunidad | texto |
| `id_del_portafolio` | ID del Portafolio | Identificador del portafolio (equivale al campo `proceso` en el dataset de documentos) | texto |
| `nombre_del_procedimiento` | Nombre del Procedimiento | Nombre otorgado por la entidad | texto |
| `descripci_n_del_procedimiento` | Descripcion | Caracteristicas principales del proceso | texto |
| `fase` | Fase | Fase actual del proceso | texto |
| `fecha_de_publicacion_del` | Fecha Publicacion Inicial | Fecha/hora inicial | timestamp |
| `fecha_de_ultima_publicaci` | Fecha Ultima Publicacion | Ultima actualizacion publicada | timestamp |
| `fecha_de_publicacion_fase` | Fecha Planeacion Precalificacion | Fecha fase planeacion | timestamp |
| `fecha_de_publicacion_fase_1` | Fecha Seleccion Precalificacion | Fecha fase seleccion precalificacion | timestamp |
| `fecha_de_publicacion` | Fecha Manifestacion de Interes | Fecha fase manifestacion | timestamp |
| `fecha_de_publicacion_fase_2` | Fecha Borrador | Fecha fase borrador | timestamp |
| `fecha_de_publicacion_fase_3` | Fecha Seleccion | Fecha fase seleccion | timestamp |
| `precio_base` | Precio Base | Valor proyectado | numero |
| `modalidad_de_contratacion` | Modalidad | Modalidad de contratacion | texto |
| `justificaci_n_modalidad_de` | Justificacion Modalidad | Justificacion de la modalidad | texto |
| `duracion` | Duracion | Duracion estimada | numero |
| `unidad_de_duracion` | Unidad de Duracion | Unidad (dias, meses, etc.) | texto |
| `fecha_de_recepcion_de` | Fecha Recepcion Respuestas | Fecha asignada a recepcion | timestamp |
| `fecha_de_apertura_de_respuesta` | Fecha Apertura Estimada | Fecha estimada de apertura | timestamp |
| `fecha_de_apertura_efectiva` | Fecha Apertura Efectiva | Fecha real de apertura | timestamp |
| `ciudad_de_la_unidad_de` | Ciudad Unidad Contratacion | Ciudad de la unidad | texto |
| `nombre_de_la_unidad_de` | Nombre Unidad | Nombre de la unidad | texto |
| `proveedores_invitados` | Proveedores Invitados | Numero de proveedores invitados | numero |
| `proveedores_con_invitacion` | Invitacion Directa | Proveedores con invitacion directa | numero |
| `visualizaciones_del` | Visualizaciones | Numero de visualizaciones | numero |
| `proveedores_que_manifestaron` | Proveedores con interes | Numero que manifestaron interes | numero |
| `respuestas_al_procedimiento` | Respuestas | Respuestas totales | numero |
| `respuestas_externas` | Respuestas Externas | Respuestas de entes externos | numero |
| `conteo_de_respuestas_a_ofertas` | Respuestas Ofertas | Numero de respuestas directas sobre ofertas | numero |
| `proveedores_unicos_con` | Proveedores Unicos | Proveedores unicos con respuestas | numero |
| `numero_de_lotes` | Numero de Lotes | Lotes de articulos | numero |
| `estado_del_procedimiento` | Estado | Estado actual del procedimiento | texto |
| `id_estado_del_procedimiento` | ID Estado | Identificador del estado | numero |
| `adjudicado` | Adjudicado | Indica si fue adjudicado | texto |
| `id_adjudicacion` | ID Adjudicacion | Identificador adjudicacion | texto |
| `codigoproveedor` | Codigo Proveedor | Codigo del proveedor adjudicado | texto |
| `departamento_proveedor` | Departamento Proveedor | Departamento del proveedor adjudicado | texto |
| `ciudad_proveedor` | Ciudad Proveedor | Ciudad del proveedor adjudicado | texto |
| `fecha_adjudicacion` | Fecha Adjudicacion | Fecha de adjudicacion | timestamp |
| `valor_total_adjudicacion` | Valor Total | Valor adjudicado | numero |
| `nombre_del_adjudicador` | Nombre Adjudicador | Usuario que adjudica | texto |
| `nombre_del_proveedor` | Proveedor Adjudicado | Nombre del proveedor adjudicado | texto |
| `nit_del_proveedor_adjudicado` | NIT Proveedor | NIT del proveedor adjudicado | texto |
| `codigo_principal_de_categoria` | Codigo Categoria | Codigo UNSPSC principal | texto |
| `estado_de_apertura_del_proceso` | Estado de Apertura | Estado de apertura de informacion | texto |
| `tipo_de_contrato` | Tipo de Contrato | Tipo de contrato | texto |
| `subtipo_de_contrato` | Subtipo | Subtipo de contrato | texto |
| `categorias_adicionales` | Categorias Adicionales | Codigos UNSPSC adicionales | texto |
| `urlproceso` | URL Proceso | URL en SECOP II | url |
| `codigo_entidad` | Codigo Entidad | Codigo interno de SECOP II | numero |
| `estado_resumen` | Estado Resumen | Resumen del estado | texto |

## 4. Dataset de documentos SECOP II
- **Endpoint JSON:** `https://www.datos.gov.co/resource/dmgg-8hin.json`
- **Relacion clave:** el campo `proceso` de este dataset corresponde a `id_del_portafolio` en el dataset principal.
- **Campos relevantes:** `id_documento`, `nombre_archivo`, `descripci_n`, `extensi_n`, `tamanno_archivo`, `fecha_carga`, `entidad`, `url_descarga_documento`.
- **Uso:** para mostrar documentos asociados a un proceso primero consulte el dataset principal, obtenga el valor de `id_del_portafolio` y utilicelo como filtro (`proceso = 'CO1.BDOS....'`).

## 5. Parametros SoQL disponibles mas usados
- `$select`: define campos y expresiones a retornar.
- `$where`: filtros logicos con comparaciones, funciones (por ejemplo `upper`, `like`, `between`, `in`).
- `$limit` y `$offset`: control de paginacion.
- `$order`: ordenamiento ascendente o descendente (`campo asc`, `campo desc`).
- `$group`, `$sum`, `$count`, `$avg`: agregaciones.
- `$q`: busqueda de texto completo en columnas indexadas.
- `$having`: filtros post-agregacion.
- `$chunked`: habilita paginacion de grandes volumenes (requiere token).

## 6. Ejemplos de consultas REST

### 6.1. Obtener los ultimos 5 procesos publicados
```
https://www.datos.gov.co/resource/p6dx-8zbt.json?$select=id_del_proceso,referencia_del_proceso,entidad,fecha_de_publicacion_del&$order=fecha_de_publicacion_del%20DESC&$limit=5
```

### 6.2. Filtrar por entidad y rango de fechas
```
https://www.datos.gov.co/resource/p6dx-8zbt.json?
  $select=id_del_proceso,entidad,estado_del_procedimiento,precio_base&
  $where=entidad='ALCALDIA MAYOR DE BOGOTA D.C.'%20AND%20fecha_de_publicacion_del%20between%20'2024-01-01T00:00:00'%20and%20'2024-12-31T23:59:59'&
  $order=fecha_de_publicacion_del%20DESC
```

### 6.3. Buscar por referencia de proceso
```
https://www.datos.gov.co/resource/p6dx-8zbt.json?$where=referencia_del_proceso='CD-123-2024'
```

### 6.4. Consultar solo procesos adjudicados con valor superior a 500 millones
```
https://www.datos.gov.co/resource/p6dx-8zbt.json?
  $select=id_del_proceso,entidad,valor_total_adjudicacion,fecha_adjudicacion&
  $where=adjudicado='Si'%20AND%20valor_total_adjudicacion>500000000&
  $order=valor_total_adjudicacion%20DESC
```

### 6.5. Contar procesos por modalidad de contratacion publicados en 2024
```
https://www.datos.gov.co/resource/p6dx-8zbt.json?
  $select=modalidad_de_contratacion,%20count(id_del_proceso)%20as%20total_procesos&
  $where=fecha_de_publicacion_del%20between%20'2024-01-01T00:00:00'%20and%20'2024-12-31T23:59:59'&
  $group=modalidad_de_contratacion&
  $order=total_procesos%20DESC
```

### 6.6. Filtrado de texto parcial (use `upper` para case-insensitive)
```
https://www.datos.gov.co/resource/p6dx-8zbt.json?$where=upper(nombre_del_procedimiento)%20like%20'CONTRATO%25'
```

### 6.7. Obtener documentos asociados a un proceso (id_del_portafolio)
```
https://www.datos.gov.co/resource/dmgg-8hin.json?
  $select=nombre_archivo,descripci_n,extensi_n,fecha_carga,url_descarga_documento&
  $where=proceso='CO1.BDOS.4161571'&
  $order=fecha_carga%20DESC
```

## 7. Ejemplos en codigo

### 7.1. PowerShell
```powershell
$endpoint = "https://www.datos.gov.co/resource/p6dx-8zbt.json"
$params = @{
    "$select" = "id_del_proceso, entidad, estado_del_procedimiento"
    "$where"  = "estado_del_procedimiento = 'Celebrado'"
    "$limit"  = 20
    "$order"  = "fecha_de_publicacion_del DESC"
}
$response = Invoke-RestMethod -Uri $endpoint -Method Get -Body $null -ContentType 'application/json' -Query $params
$response
```

### 7.2. Python (requests)

```python
import requests

ENDPOINT = "https://www.datos.gov.co/resource/p6dx-8zbt.json"
params = {
    "$select": "id_del_proceso, referencia_del_proceso, entidad, valor_total_adjudicacion",
    "$where": "adjudicado = 'Si' AND valor_total_adjudicacion > 0",
    "$order": "fecha_adjudicacion DESC",
    "$limit": 50
}
headers = {"Accept": "application/json"}
resp = requests.get(ENDPOINT, params=params, headers=headers, timeout=30)
resp.raise_for_status()
data = resp.json()
```

### 7.3. Python (documentos asociados)
```python
import requests

DOCS_ENDPOINT = "https://www.datos.gov.co/resource/dmgg-8hin.json"
params = {
  "$select": "nombre_archivo, descripci_n, url_descarga_documento",
  "$where": "proceso = 'CO1.BDOS.4161571'",
  "$order": "fecha_carga DESC",
  "$limit": 10,
}
resp = requests.get(DOCS_ENDPOINT, params=params, timeout=30)
resp.raise_for_status()
documentos = resp.json()
```

## 8. Consideraciones de calidad de datos
- Fechas pueden venir vacias en procesos sin avance; valide nulos antes de convertir a objetos `datetime`.
- Campos numericos se entregan como texto cuando el valor es nulo; convierta usando manejo de excepciones.
- Algunos textos contienen tildes y caracteres especiales; asegure codificacion UTF-8 en la aplicacion consumidora.
- Los campos `adjudicado` y `estado_del_procedimiento` manejan valores normalizados (`Si`/`No`, `Celebrado`, `Borrador`, etc.). Considere crear diccionarios de referencia.
- Para obtener documentos asociados use `id_del_portafolio`; este valor corresponde al campo `proceso` en el dataset `dmgg-8hin`.

## 9. Recursos adicionales
- Portal del dataset: `https://www.datos.gov.co/Gastos-Gubernamentales/Procesos-de-contrataci-n-SECOP-II/p6dx-8zbt`
- Documentacion Socrata API: `https://dev.socrata.com/docs/`
- Guia SoQL completa: `https://dev.socrata.com/docs/functions/`

Con esta documentacion puede construir filtros y consultas complejas sobre procesos, fechas, referencias, adjudicaciones y otros atributos del SECOP II.
