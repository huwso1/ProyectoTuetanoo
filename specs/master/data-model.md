# Data Model: Licitaciones Inteligentes

## Entities

### Usuario
- id: UUID
- nombre: string
- correo: string (PII, encrypted)
- telefono: string (PII, encrypted)
- rol: enum [mipyme, consultor, admin]
- cuenta: enum [freemium, paid]
- consentimiento_pii: boolean
- created_at, updated_at

### Empresa
- id: UUID
- RUP: string (encrypted)
- NIT: string (encrypted)
- descripcion: text
- actividad_unspsc: string
- status: enum
- verificado: boolean

### Proyecto (Licitacion-Usuario)
- id: UUID
- referencia_secop: string
- titulo: string
- requisitos_raw: jsonb
- fechas: start_date, end_date
- documentos: relation to Document

### Documento
- id: UUID
- tipo: enum [DID, RUP, otro]
- archivo_url: string (object storage reference)
- metadatos: jsonb
- estado: enum [subido, pendiente, procesado]
- parsed_structure: jsonb (anonymized extract)

### Score/Matching
- id: UUID
- proyecto_id: UUID
- empresa_id: UUID
- score: integer (0-100)
- gaps: jsonb
- confidence: enum [alta, media, baja]
- created_at

## Validation rules
- Email, telefono y NIT solo almacenados si `consentimiento_pii=true`.
- RUP y NIT cifrados en resto y no indexados en texto sin autorización.

## Relationships
- Usuario -> Empresa (0..n)
- Empresa -> Proyecto (0..n)
- Proyecto -> Documento (0..n)
- Score references Empresa & Proyecto

## Notes
- Consider application-layer encryption for RUP/NIT if regulatory constraints demand.
- parsed_structure must be anonymized before storage unless consented.
