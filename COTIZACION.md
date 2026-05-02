# Ruido App — Plan de Desarrollo y Estimación de Costos

**Cliente:** [Nombre del cliente]
**Fecha:** Abril 2026
**Versión:** 1.0

---

## 1. Descripción General del Proyecto

**Ruido App** es una aplicación web responsiva orientada a técnicos y profesionales ambientales que realizan mediciones de ruido ambiental en campo. Su objetivo principal es digitalizar y automatizar el flujo de trabajo que hoy se ejecuta manualmente sobre plantillas Excel, generando informes oficiales que cumplen con la normativa colombiana vigente: **Resolución 0627 de 2006 del MAVDT**, Resolución 8321 de 1983 y Decreto 948 de 1995.

La plataforma permite registrar mediciones, gestionar dispositivos de medición y sus calibraciones, administrar clientes y sitios de monitoreo, calcular automáticamente los indicadores acústicos exigidos por la norma (Leq, L90, L50, L10, incertidumbre), y generar los informes en los formatos oficiales `24-001-A30_` y `FO-GA-17`.

### Estado actual del proyecto

El proyecto cuenta con una base funcional ya construida:

- Proyecto Laravel 11 inicializado con autenticación (Laravel Breeze)
- Sistema de diseño base con tokens Tailwind CSS y layouts generales
- Sección 1 del formulario de medición: "Información General" (contrato, fechas, responsables, ubicación, marco legal)
- Mockup HTML interactivo con croquis drag-and-drop (Konva.js) que cubre las 7 secciones del formulario

Esta base no se cotiza en el presente documento, pero reduce los tiempos de análisis y diseño de las fases siguientes.

---

## 2. Stack Tecnológico

| Capa | Tecnología |
|------|-----------|
| Backend | Laravel 11 (PHP 8.3) |
| Frontend | Vue 3 (Composition API, `<script setup>`) |
| Puente frontend-backend | Inertia.js |
| Estilos | Tailwind CSS |
| Base de datos | MySQL 8 / PostgreSQL 16 |
| Lectura de Excel | PhpSpreadsheet |
| Gráficos / croquis | Konva.js |
| Autenticación | Laravel Breeze (sesiones) |
| Control de versiones | Git / GitHub |
| Servidor | AWS EC2 (Ubuntu 24 LTS) |
| CI básico | GitHub Actions |
| Asistencia de desarrollo | Claude Code (Anthropic) |

> **Nota sobre productividad:** el desarrollo usa Claude Code como asistente de programación bajo la dirección de un tech lead senior. Este modelo reduce los tiempos de implementación en un 40–60% frente a un equipo de desarrollo tradicional, lo que se refleja directamente en los costos del proyecto.

---

## 3. Funcionalidades a Desarrollar

### 3.1 Relevamiento de requerimientos
Talleres de trabajo con el cliente para documentar los flujos exactos del negocio, casos de uso por rol, reglas de validación regulatoria y criterios de aceptación por funcionalidad. Entregable: documento de requerimientos funcionales y no funcionales.

### 3.2 Diseño de la base de datos
Modelado relacional completo del sistema (entidades, relaciones, índices, restricciones de integridad). Entregables: diagrama ER, migraciones Laravel y seeds iniciales para datos de catálogo (departamentos, municipios, fuentes de ruido, categorías normativas).

### 3.3 Diseño del formulario completo
Implementación de las secciones 2 a 7 del formulario `24-001-A30_` y del formulario `FO-GA-17`, con todos los campos regulatorios, validaciones en frontend y backend, guardado de borradores, y navegación multi-paso. Se apoya en el mockup HTML existente.

### 3.4 Módulo de dispositivos de medición
CRUD completo de equipos (sonómetros, calibradores acústicos), con ficha técnica por dispositivo, registro de certificados de calibración, alertas de vencimiento de calibración y asignación de equipos a mediciones activas.

### 3.5 Módulo de registro de empresas
Gestión de clientes y contratantes: NIT, razón social, representante legal, datos de contacto, y administración de sus sitios de monitoreo (dirección, coordenadas, clasificación de zona según Res. 0627).

### 3.6 Módulo de generación y gestión de informes
Flujo completo de ciclo de vida de un informe: creación desde borrador, revisión interna, emisión y aprobación. Repositorio consultable con filtros por cliente, fecha, técnico y estado. Vista de detalle e historial de cambios por informe.

### 3.7 Lectura de archivos Excel
Importación de resultados desde dos plantillas Excel específicas ya definidas: el archivo del sonómetro (datos de medición por intervalo) y el informe meteorológico. Los datos importados se asocian automáticamente al informe en curso.

### 3.8 Análisis y cálculo automático de fórmulas
Motor de cálculo que procesa los datos importados y registrados manualmente para obtener: nivel equivalente (Leq), percentiles L90, L50, L10, correcciones por impulsividad y tonalidad, e incertidumbre expandida de medición, conforme a los procedimientos de la Resolución 0627 de 2006.

### 3.9 Módulo de administración de usuarios
Gestión de cuentas con tres roles: **Administrador** (acceso total), **Técnico** (registro y edición de mediciones propias) y **Cliente** (consulta de sus propios informes). Incluye alta/baja de usuarios, asignación de roles y log de actividad básico.

### 3.10 Administración del repositorio GitHub
Configuración del repositorio con gitflow (ramas `main`, `develop`, `feature/*`, `release/*`), plantillas de PR, flujo de CI básico con GitHub Actions (lint, tests unitarios), y documentación técnica del proyecto (README, guía de instalación local, variables de entorno).

### 3.11 Despliegue en AWS
Aprovisionamiento de la infraestructura en AWS: instancia EC2 (Ubuntu 24 LTS) con Nginx + PHP-FPM, base de datos gestionada en RDS (MySQL 8), almacenamiento de archivos en S3, gestión de DNS con Route 53, certificado SSL con ACM (gratuito), Security Groups, IAM roles con mínimo privilegio, variables de entorno en Parameter Store y procedimiento de deploy documentado (GitHub Actions → EC2).

### 3.12 Infraestructura en la nube (12 meses)
Costos operativos del primer año. Se contemplan tres proveedores según prioridad del cliente: **AWS** (mayor madurez y servicios gestionados), **DigitalOcean** (interfaz simple, marketplace con Laravel preinstalado) y **Hetzner** (mayor relación costo/rendimiento). Los tres corren Ubuntu 24 LTS con Nginx + PHP-FPM + MySQL y el procedimiento de deploy es idéntico entre ellos.

### 3.13 Generación del documento final
Exportación del informe de medición terminado en el formato oficial `24-001-A30_`, rellenando automáticamente la plantilla Excel entregada por el cliente con todos los datos registrados en el sistema (información general, resultados de medición, cálculos, croquis y firma del profesional). Incluye también exportación en PDF listo para entregar al contratante o a la autoridad ambiental.

### 3.14 Módulo del profesional de medición
Registro y administración de los profesionales que ejecutan las mediciones: nombre completo, número de tarjeta profesional, especialidad, vigencia de certificaciones, firma digitalizada y documentos de soporte. El profesional se asocia a cada informe para que su información y firma queden incluidas en el documento final generado.

### 3.15 Soporte post-lanzamiento (6 meses)
Mantenimiento correctivo (corrección de errores reportados) y preventivo (actualizaciones de dependencias, monitoreo de disponibilidad, ajustes menores de configuración) durante los seis meses posteriores al lanzamiento oficial.

---

## 4. Plan de Fases de Desarrollo

### Fase 0 — Relevamiento y Arquitectura (Semanas 1–2)
Talleres con el cliente, revisión de las plantillas Excel existentes, definición del modelo de datos y documentación de requerimientos. Al final de esta fase el equipo tiene claridad total sobre el alcance antes de escribir una sola línea de código de negocio.

### Fase 1 — Base de datos y módulos de catálogo (Semanas 3–4)
Diseño y creación del esquema relacional completo: migraciones, seeds, relaciones Eloquent. Módulos de empresas y dispositivos de medición (CRUD, calibraciones).

### Fase 2 — Formulario de medición completo (Semanas 5–8)
Implementación de las secciones 2 a 7 del formulario `24-001-A30_` y el formulario `FO-GA-17`. Incluye guardado de borradores, validaciones regulatorias, croquis interactivo y carga de fotos de campo.

### Fase 3 — Importación Excel y motor de cálculo (Semanas 9–11)
Lector de archivos Excel para los dos templates definidos. Motor de cálculo de indicadores acústicos (Leq, percentiles, incertidumbre) con visualización de resultados en el informe.

### Fase 4 — Gestión de informes, profesionales y usuarios (Semanas 12–14)
Ciclo de vida del informe (borrador → emitido → aprobado), repositorio con búsqueda y filtros, módulo de administración de usuarios con roles y permisos, módulo del profesional de medición (registro, certificaciones, firma digitalizada).

### Fase 5 — Generación del documento final (Semanas 15–16)
Exportación del informe completo en la plantilla Excel oficial `24-001-A30_` con todos los datos calculados y la firma del profesional. Exportación a PDF. Pruebas de fidelidad contra los formatos regulatorios de referencia.

### Fase 6 — Despliegue en AWS y entrega (Semanas 17–18)
Aprovisionamiento de servicios AWS (EC2, RDS, S3, Route 53), configuración de Nginx + PHP-FPM, pipeline de deploy con GitHub Actions, variables de entorno en AWS Parameter Store, pruebas de aceptación con el cliente y capacitación de usuarios finales.

**Duración total estimada: 18 semanas (~4,5 meses)**

---

## 5. Estimación de Costos

La tarifa base de desarrollo es **$350.000 COP por día efectivo de trabajo** (jornadas de 4–6 horas dirigidas por un tech lead senior con asistencia de Claude Code). Esta tarifa refleja el modelo de alta productividad con IA, significativamente por debajo de una tarifa de mercado para un desarrollador senior contratado a tiempo completo.

### 5.1 Desarrollo de software

| # | Funcionalidad / Entregable | Días estimados | Costo COP |
|---|---------------------------|:--------------:|----------:|
| 1 | Relevamiento de requerimientos (talleres + doc) | 5 | $1.750.000 |
| 2 | Diseño y modelado de base de datos + migraciones + seeds | 6 | $2.100.000 |
| 3 | Formulario completo `24-001-A30_` (secciones 2–7) | 14 | $4.900.000 |
| 4 | Formulario `FO-GA-17` | 5 | $1.750.000 |
| 5 | Módulo de dispositivos de medición (CRUD + calibraciones) | 5 | $1.750.000 |
| 6 | Módulo de registro de empresas y sitios de monitoreo | 4 | $1.400.000 |
| 7 | Módulo de gestión de informes (ciclo de vida + repositorio) | 7 | $2.450.000 |
| 8 | Lectura e importación de archivos Excel (2 templates) | 6 | $2.100.000 |
| 9 | Motor de cálculo automático (Leq, L90, L50, L10, incertidumbre) | 8 | $2.800.000 |
| 10 | Módulo de administración de usuarios y roles | 4 | $1.400.000 |
| 11 | Módulo del profesional de medición (registro, firma, certificaciones) | 4 | $1.400.000 |
| 12 | Generación del documento final (Excel oficial + PDF exportable) | 9 | $3.150.000 |
| 13 | Administración GitHub (gitflow, CI, documentación técnica) | 3 | $1.050.000 |
| 14 | Despliegue en AWS (EC2, RDS, S3, Route 53, CI/CD, IAM) | 4 | $1.400.000 |
| | **Subtotal desarrollo** | **84 días** | **$29.400.000** |

### 5.2 Infraestructura en la nube (12 meses)

Costos estimados a **TRM $4.200 COP/USD**. Se presentan tres opciones; el cliente elige según sus preferencias de proveedor, presupuesto y nivel de control técnico requerido.

#### Opción A — AWS ⭐ Mayor madurez empresarial

Región recomendada: `sa-east-1` (São Paulo) para menor latencia desde Colombia.

| Servicio | Especificación | USD/mes | COP/año |
|---------|---------------|--------:|--------:|
| EC2 `t3.small` | App server — 2 vCPU, 2 GB RAM | ~$17 | $856.800 |
| RDS `db.t3.micro` | MySQL 8 gestionado, 20 GB SSD | ~$20 | $1.008.000 |
| S3 Standard | Archivos, informes, fotos (~20 GB) | ~$5 | $252.000 |
| Route 53 + dominio | DNS + renovación anual | ~$2 | $130.400 |
| ACM SSL | Certificado SSL/TLS | Gratis | — |
| **Subtotal Opción A** | | **~$44/mes** | **$2.247.200** |

> Complejidad de configuración: **Alta** — requiere gestión de IAM, VPC, Security Groups y múltiples servicios.
> Año 1 con Free Tier (t2.micro + db.t3.micro): costo reducido a ~**$382.400 COP** (solo S3 + dominio).

---

#### Opción B — DigitalOcean ⭐ Interfaz simple, ideal para equipos pequeños

Datacenter recomendado: NYC3 o SFO3 (sin datacenter en Latinoamérica actualmente).

| Servicio | Especificación | USD/mes | COP/año |
|---------|---------------|--------:|--------:|
| Droplet Basic | 2 vCPU, 2 GB RAM, 60 GB SSD | ~$18 | $907.200 |
| Managed MySQL | 1 nodo básico, 1 GB RAM | ~$15 | $756.000 |
| Spaces (S3-compatible) | 250 GB + transferencia | ~$5 | $252.000 |
| Dominio + DNS | Registro anual | — | $130.000 |
| SSL | Let's Encrypt (gratis) | Gratis | — |
| **Subtotal Opción B** | | **~$38/mes** | **$2.045.200** |

> Complejidad de configuración: **Media** — panel muy intuitivo, marketplace con Laravel preinstalado en 1 clic, documentación excelente en español.

---

#### Opción C — Hetzner ⭐ Mejor relación costo/rendimiento

Datacenter recomendado: Hillsboro, EE.UU. (menor latencia desde Colombia vs. Europa).

| Servicio | Especificación | USD/mes | COP/año |
|---------|---------------|--------:|--------:|
| VPS `CX21` | 2 vCPU AMD, 4 GB RAM, 40 GB SSD | ~$6 | $302.400 |
| MySQL en mismo servidor | Sin costo adicional | — | — |
| Hetzner Object Storage | S3-compatible, 1 TB incluido | ~$6 | $302.400 |
| Dominio + DNS | Registro anual | — | $130.000 |
| SSL | Let's Encrypt (gratis) | Gratis | — |
| **Subtotal Opción C** | | **~$12/mes** | **$734.800** |

> Complejidad de configuración: **Media-baja** — VPS puro sin servicios extra, menos opciones pero menos burocracia. La BD corre en el mismo servidor (suficiente para esta escala).

---

#### Comparativo de opciones

| | Opción A — AWS | Opción B — DigitalOcean | Opción C — Hetzner |
|-|:-:|:-:|:-:|
| Costo anual estimado | $2.247.200 COP | $2.045.200 COP | $734.800 COP |
| Complejidad de config. | Alta | Media | Media-baja |
| Facilidad de uso | ★★☆ | ★★★ | ★★☆ |
| Madurez / SLA | ★★★ | ★★★ | ★★☆ |
| Soporte en español | ★★☆ | ★★★ | ★★☆ |
| Datacenter en Latam | Sí (São Paulo) | No | No (EE.UU. más cercano) |

> **Recomendación:** para este proyecto en etapa inicial, **Hetzner (Opción C)** ofrece el mayor ahorro sin sacrificar rendimiento. Si el cliente exige infraestructura AWS por política corporativa o requisitos de cumplimiento, usar **AWS Free Tier el primer año** y migrar a instancias reservadas desde el año 2.

### 5.3 Soporte post-lanzamiento (6 meses)

| Concepto | Costo COP |
|----------|----------:|
| Mantenimiento correctivo (hasta 2 incidencias/mes) | $1.500.000 |
| Mantenimiento preventivo (actualizaciones, monitoreo) | $900.000 |
| Ajustes menores y consultas técnicas | $600.000 |
| **Subtotal soporte** | **$3.000.000** |

### 5.4 Resumen General

| Categoría | AWS (A) | DigitalOcean (B) | Hetzner (C) |
|-----------|--------:|----------------:|------------:|
| Desarrollo de software (84 días) | $29.400.000 | $29.400.000 | $29.400.000 |
| Infraestructura — 12 meses | $2.247.200 | $2.045.200 | $734.800 |
| Soporte post-lanzamiento — 6 meses | $3.000.000 | $3.000.000 | $3.000.000 |
| **TOTAL ANTES DE IMPUESTOS** | **$34.647.200** | **$34.445.200** | **$33.134.800** |
| Con IVA (19%) | ~$41.230.000 | ~$40.990.000 | ~$39.430.000 |

> Los costos de infraestructura están expresados en USD y se facturan mensualmente según consumo real. Los valores en COP son estimados a TRM $4.200 y variarán con la tasa de cambio.
>
> Con **AWS Free Tier** el primer año, el total baja a aproximadamente **$32.782.400 COP** (Opción A, año 1).

---

## 6. Notas, Supuestos y Exclusiones

### Supuestos

- El cliente provee acceso oportuno a: los templates Excel de los equipos de medición, los formatos regulatorios de referencia y un usuario experto en el proceso para validar los cálculos.
- El dominio será registrado o transferido por el cliente; el costo listado asume que el proveedor es el mismo equipo de desarrollo.
- Las pruebas de aceptación serán realizadas por el cliente dentro de los 10 días hábiles siguientes a cada entrega de fase.
- La generación de informes en este alcance es en pantalla y exportación de datos. La **generación de PDF fiel al formato oficial** (`24-001-A30_`) es una funcionalidad adicional no incluida en este presupuesto.
- No se incluye aplicación móvil nativa (iOS/Android). La aplicación es web responsiva, accesible desde dispositivos móviles mediante el navegador.
- El plan de soporte cubre errores de funcionamiento y ajustes menores; los cambios de alcance o nuevas funcionalidades se cotizarán por separado.

### Exclusiones

- Firma digital de documentos con validez jurídica (e.firma, token USB, FirmaEC u equivalente) — la firma digitalizada del profesional es una imagen escaneada, no firma electrónica certificada.
- Integración con sistemas externos (ERP del cliente, APIs gubernamentales, pasarelas de pago).
- Licencias de software de terceros distintas a las mencionadas en el stack (todas las tecnologías listadas son open source o de uso libre).
- Capacitación formal en aula (se incluye una sesión de presentación/entrenamiento básico; capacitaciones adicionales se cobran por separado).
- Seguro de responsabilidad civil o pólizas de cumplimiento (aplica si el contrato lo exige).

### Condiciones comerciales

- **Forma de pago sugerida:** 30% al inicio, 40% al completar la Fase 3 (motor de cálculo), 30% al cierre y despliegue.
- **Validez de la oferta:** 30 días calendario desde la fecha de emisión.
- **Moneda:** Pesos colombianos (COP). Los costos de infraestructura en USD están sujetos a la TRM vigente al momento de la facturación.

---

## 7. Sobre el Modelo de Desarrollo

Este proyecto usa un modelo de desarrollo no convencional: **un tech lead senior que dirige Claude Code** (modelo de IA de Anthropic) como asistente de programación de alta capacidad. Este modelo ofrece al cliente tres ventajas concretas:

1. **Velocidad:** tareas que a un desarrollador junior/semi-senior le toman 2–3 días se resuelven en 4–8 horas de trabajo enfocado.
2. **Costo:** la tarifa diaria es un 50–65% menor que contratar un desarrollador senior en el mercado colombiano ($350.000/día vs. $800.000–$1.200.000/día de un dev senior contratado).
3. **Calidad:** el tech lead mantiene el criterio arquitectónico, revisa todo el código generado y garantiza que las decisiones de diseño sean sólidas y mantenibles a largo plazo.

El riesgo de este modelo es la dependencia de una sola persona con criterio técnico. Se mitiga con documentación exhaustiva del código, cobertura de pruebas unitarias en los módulos críticos (motor de cálculo) y la entrega del repositorio completo al cliente al finalizar el proyecto.

---

*Documento preparado por el equipo técnico del proyecto. Para consultas o ajustes de alcance, contactar al tech lead responsable.*
