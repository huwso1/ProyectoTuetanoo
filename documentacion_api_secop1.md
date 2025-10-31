# Documentacion API SECOP I (datos.gov.co)

## 1. Introduccion
El dataset `f789-7hwg` expone informacion consolidada de procesos de contratacion publicados en SECOP I. Se publica a traves de la plataforma Socrata de datos abiertos de Colombia y se consulta mediante peticiones REST compatibles con el lenguaje SoQL.

- **Endpoint base JSON:** `https://www.datos.gov.co/resource/f789-7hwg.json`
- **Formato de respuesta:** JSON (UTF-8)
- **Autenticacion:** Publica. Para volumenes altos registrar un `app_token` y enviarlo en el encabezado `X-App-Token`.

## 2. Buenas practicas
- Limitar resultados con `$limit` (maximo 5000 sin token, 50000 con token) y paginar con `$offset`.
- Ordenar con `$order` para obtener listas deterministicas.
- Normalizar fechas en ISO 8601 cuando se usen en `$where` (`YYYY-MM-DDThh:mm:ss`).
- Manejar valores `No Definido` o campos nulos antes de convertirlos a tipos estrictos.
- Revisar el portal del dataset para cambios en el esquema o valores de referencia.

## 3. Mapeo de campos clave
| Campo API | Nombre legible | Descripcion | Tipo |
|-----------|----------------|-------------|------|
| `uid` | UID | Identificador unico del registro (constancia-adjudicacion) | texto |
| `anno_cargue_secop` | Año cargue | Año en que el proceso se registro en SECOP I | numero |
| `anno_firma_contrato` | Año firma contrato | Año de firma del contrato (texto, puede ser "No firmado") | texto |
| `nivel_entidad` | Nivel entidad | Nivel (Nacional/Territorial) | texto |
| `orden_entidad` | Orden entidad | Clasificacion detallada de la entidad | texto |
| `nombre_entidad` | Entidad | Nombre de la entidad compradora | texto |
| `nit_de_la_entidad` | NIT entidad | NIT de la entidad compradora | texto |
| `c_digo_de_la_entidad` | Codigo entidad | Codigo SECOP I de la entidad | numero |
| `id_modalidad` | ID modalidad | Identificador numerico de la modalidad | numero |
| `modalidad_de_contratacion` | Modalidad | Modalidad de seleccion del proceso | texto |
| `estado_del_proceso` | Estado proceso | Estado en SECOP I (Convocado, Celebrado, etc.) | texto |
| `causal_de_otras_formas_de` | Causal | Causal de contratacion directa, si aplica | texto |
| `id_regimen_de_contratacion` | ID regimen | Identificador numerico del regimen | numero |
| `nombre_regimen_de_contratacion` | Regimen | Nombre del regimen (Ley 80, regimen especial, etc.) | texto |
| `id_objeto_a_contratar` | ID objeto | Codigo UNSPSC del objeto | numero |
| `objeto_a_contratar` | Objeto | Descripcion UNSPSC del objeto | texto |
| `detalle_del_objeto_a_contratar` | Detalle objeto | Descripcion libre del bien/servicio | texto |
| `tipo_de_contrato` | Tipo contrato | Categoria amplia del contrato | texto |
| `municipio_de_obtencion` | Municipio obtencion | Municipio donde se desarrolla el proceso | texto |
| `municipio_de_entrega` | Municipio entrega | Municipio de entrega | texto |
| `municipios_ejecucion` | Municipios ejecucion | Municipios donde se ejecuta el contrato | texto |
| `fecha_de_cargue_en_el_secop` | Fecha cargue | Fecha de registro en SECOP I | timestamp |
| `numero_de_constancia` | Numero constancia | Identificador generado por SECOP I | texto |
| `numero_de_proceso` | Numero proceso | Identificador definido por la entidad | texto |
| `numero_de_contrato` | Numero contrato | Identificador del contrato | texto |
| `cuantia_proceso` | Cuantia proceso | Valor del proceso | numero |
| `plazo_de_ejec_del_contrato` | Plazo ejecucion | Duracion del contrato | numero |
| `rango_de_ejec_del_contrato` | Unidad plazo | Unidad del plazo (dias, meses) | texto |
| `valor_contrato_con_adiciones` | Valor contrato total | Valor del contrato con adiciones | numero |
| `objeto_del_contrato_a_la` | Objeto contrato | Objeto al momento de la firma | texto |
| `ruta_proceso_en_secop_i` | URL proceso | Ruta web del proceso en SECOP I | url |
| `municipio_entidad` | Municipio entidad | Municipio de la entidad compradora | texto |
| `departamento_entidad` | Departamento entidad | Departamento de la entidad | texto |
| `ultima_actualizacion` | Ultima actualizacion | Timestamp de la ultima modificacion | timestamp |
| `cumpledecreto248` | Decreto 248 | Marcador de cumplimiento Decreto 248 (texto) | texto |
| `incluyebienesdecreto248` | Incluye bienes Decreto 248 | Identifica si incluye bienes del decreto | texto |
| `cumple_sentencia_t302` | Sentencia T302 | Marcador de cumplimiento sentencia T302 | texto |
| `es_mipyme` | Es MiPyme | Indica si la entidad es MiPyme | texto |
| `tama_o_mipyme` | Tamano MiPyme | Clasificacion de tamaño de la entidad | texto |

*(La tabla completa incluye mas de 70 campos; revise el portal oficial para detalles adicionales.)*

## 4. Dataset de documentos SECOP I
- **Endpoint:** `https://www.datos.gov.co/resource/ps88-5e3v.json`
- **Relacion clave:** `numero_de_constancia` enlaza documentos con procesos del dataset principal.
- **Campos relevantes:** `nombrearchivo`, `descripcion`, `extension`, `tama_o`, `ruta_descarga`, `fecha_ultima_modificacion`.

## 5. Parametros SoQL utiles
- `$select`, `$where`, `$order`, `$limit`, `$offset` como en SECOP II.
- `$group`, `$count`, `$sum`, `$avg` para agregaciones.
- `$q` para busquedas de texto completo.
- Funciones como `upper`, `like`, `between`, `extract` disponibles en SoQL.

## 6. Ejemplos de consultas REST

### 6.1. Ultimos 5 procesos actualizados
```
https://www.datos.gov.co/resource/f789-7hwg.json?
  $select=numero_de_constancia,nombre_entidad,estado_del_proceso,ultima_actualizacion&
  $order=ultima_actualizacion%20DESC&
  $limit=5
```

### 6.2. Filtrar por entidad y estado
```
https://www.datos.gov.co/resource/f789-7hwg.json?
  $select=numero_de_constancia,numero_de_proceso,cuantia_proceso,estado_del_proceso&
  $where=nombre_entidad='TOLIMA - ALCALDIA MUNICIPIO DE SAN LUIS'%20AND%20estado_del_proceso='CONVOCADO'&
  $order=fecha_de_cargue_en_el_secop%20DESC
```

### 6.3. Buscar por numero de constancia
```
https://www.datos.gov.co/resource/f789-7hwg.json?$where=numero_de_constancia='19-4-8885396'
```

### 6.4. Procesos de 2024 con cuantia mayor a 500 millones
```
https://www.datos.gov.co/resource/f789-7hwg.json?
  $select=numero_de_constancia,nombre_entidad,cuantia_proceso,estado_del_proceso&
  $where=anno_cargue_secop=2024%20AND%20cuantia_proceso>500000000
```

### 6.5. Contar procesos por modalidad en 2024
```
https://www.datos.gov.co/resource/f789-7hwg.json?
  $select=modalidad_de_contratacion,count(numero_de_constancia)%20as%20total&
  $where=anno_cargue_secop=2024&
  $group=modalidad_de_contratacion&
  $order=total%20DESC
```

### 6.6. Documentos asociados a una constancia
```
https://www.datos.gov.co/resource/ps88-5e3v.json?
  $select=nombrearchivo,descripcion,extension,ruta_descarga,fecha_ultima_modificacion&
  $where=numero_de_constancia='25-13-14548070'&
  $order=fecha_ultima_modificacion%20DESC
```

## 7. Ejemplos en codigo

### 7.1. Python (requests)
```python
import requests

BASE = "https://www.datos.gov.co/resource/f789-7hwg.json"
params = {
    "$select": "numero_de_constancia, nombre_entidad, cuantia_proceso",
    "$where": "estado_del_proceso = 'CONVOCADO' AND anno_cargue_secop = 2025",
    "$order": "ultima_actualizacion DESC",
    "$limit": 20,
}
resp = requests.get(BASE, params=params, timeout=30)
resp.raise_for_status()
procesos = resp.json()
```

### 7.2. PowerShell
```powershell
$base = "https://www.datos.gov.co/resource/f789-7hwg.json"
$params = @{
    "$select" = "numero_de_constancia, nombre_entidad, estado_del_proceso"
    "$where"  = "nombre_entidad = 'ARAUCA - E.S.E. MORENO Y CLAVIJO - ARAUCA'"
    "$order"  = "fecha_de_cargue_en_el_secop DESC"
    "$limit"  = 10
}
$result = Invoke-RestMethod -Uri $base -Method Get -Body $null -Query $params
$result
```

### 7.3. Documentos en Python
```python
DOCS = "https://www.datos.gov.co/resource/ps88-5e3v.json"
params = {
    "$select": "nombrearchivo, descripcion, ruta_descarga",
    "$where": "numero_de_constancia = '25-13-14548070'",
    "$order": "fecha_ultima_modificacion DESC",
    "$limit": 5,
}
resp = requests.get(DOCS, params=params, timeout=30)
resp.raise_for_status()
docs = resp.json()
```

## 8. Consideraciones
- Los campos de fecha `DDMMYYYY` son convertidos por Socrata a timestamps ISO; si no hay datos se reciben como nulos.
- Muchos campos de texto usan mayusculas y caracteres especiales; asegure manejo en UTF-8.
- La cuantia y otros campos numericos vienen como strings cuando son nulos; castee con cuidado.
- Valores "No Definido" se usan como texto literal para campos sin informacion.

## 9. Recursos
- Portal del dataset SECOP I: `https://www.datos.gov.co/Gastos-Gubernamentales/Procesos-de-contrataci-n-SECOP-I/f789-7hwg`
- Dataset de documentos: `https://www.datos.gov.co/Gastos-Gubernamentales/Archivos-SECOP-I/ps88-5e3v`
- Documentacion Socrata API / SoQL: `https://dev.socrata.com/docs/`

Esta guia cubre los campos principales, consultas de ejemplo y buenas practicas para integrar datos de SECOP I, incluyendo la descarga de documentos asociados mediante `numero_de_constancia`.
