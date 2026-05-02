# Ruido App — Propuesta de Desarrollo y Estimación de Costos

**Cliente:** [Nombre del cliente]
**Fecha:** Abril 2026
**Versión:** 1.0

---

## 1. ¿Qué es Ruido App y para qué sirve?

Hoy en día, cuando un técnico realiza una medición de ruido en campo, el proceso de elaborar el informe final es manual: toma los datos del sonómetro, los pasa a una planilla Excel, hace los cálculos a mano o con fórmulas dispersas, dibuja el croquis del lugar en papel o en otro programa, y arma el documento final. Ese proceso tarda horas, es propenso a errores y depende completamente del conocimiento individual de cada técnico.

**Ruido App** es una aplicación web que reemplaza ese proceso. El técnico abre la aplicación desde cualquier computador o celular, llena el formulario paso a paso, carga los archivos que genera el sonómetro, y el sistema hace los cálculos automáticamente y genera el informe final listo para entregar, cumpliendo con la **Resolución 0627 de 2006** y demás normativa colombiana vigente.

En resumen: **lo que hoy toma varias horas de trabajo manual, con la aplicación toma minutos.**

### ¿Qué tiene avanzado el proyecto?

Antes de esta propuesta ya se construyó una base de trabajo que no se cobra aquí, pero que reduce el tiempo y costo del desarrollo:

- La aplicación web ya existe y tiene sistema de ingreso con usuario y contraseña.
- Ya tiene un diseño visual base consistente (colores, tipografía, botones).
- Ya tiene construida la primera sección del formulario de medición (Información General).
- Ya existe un prototipo interactivo del croquis de ubicación donde se pueden arrastrar y soltar elementos como mesas, puertas, personas y fuentes de ruido.

---

## 2. Herramientas que se usarán para construir la aplicación

No es necesario entender estos términos en detalle, pero se listan para transparencia técnica. Todas las tecnologías son de uso libre y gratuito (no implican licencias adicionales).

| ¿Para qué sirve? | Herramienta |
|-----------------|------------|
| Lógica del servidor (lo que el usuario no ve) | Laravel 11 (PHP 8.3) |
| Lo que el usuario ve en pantalla | Vue 3 |
| Base de datos (donde se guardan todos los datos) | MySQL 8 |
| Lectura de archivos Excel del sonómetro y meteorología | PhpSpreadsheet |
| Croquis interactivo de ubicación | Konva.js |
| Control y seguridad del ingreso de usuarios | Laravel Breeze |
| Guardado de archivos (informes, fotos, Excel) | Amazon S3 / equivalente |
| Historial de cambios del código fuente | Git / GitHub |
| Asistente de programación con inteligencia artificial | Claude Code (Anthropic) |

> **¿Por qué se usa inteligencia artificial para programar?**
> El desarrollo usa Claude Code, un asistente de IA, bajo la supervisión directa de un desarrollador senior. Esto permite construir más rápido y a menor costo: tareas que normalmente toman 2 o 3 días se resuelven en horas. El desarrollador revisa y valida todo lo que produce la IA antes de incorporarlo al proyecto. El resultado es un costo de desarrollo entre 50% y 65% menor que el de un equipo tradicional, sin sacrificar calidad.

---

## 3. ¿Qué se va a construir? — Descripción de cada módulo

### 3.1 Relevamiento de requerimientos
**¿Qué es?** Antes de escribir una sola línea de código, el equipo se sienta con los técnicos y responsables de la empresa para entender exactamente cómo trabajan hoy, qué necesitan que haga la aplicación, y cuáles son los criterios para decir que algo "quedó bien hecho".

**¿Qué produce?** Un documento escrito con todas las funcionalidades acordadas, los flujos de trabajo por tipo de usuario y los criterios de aprobación de cada entrega. Este documento es el mapa que guía todo el desarrollo.

**¿Por qué es importante?** Porque los cambios que se hacen antes de programar cuestan $0. Los cambios que se hacen a mitad del desarrollo pueden costar 5 o 10 veces más.

---

### 3.2 Diseño de la base de datos
**¿Qué es?** La base de datos es el "archivo" digital donde la aplicación guarda toda la información: los informes, los técnicos, las empresas, los dispositivos, las mediciones, etc. Diseñarla bien desde el inicio es fundamental para que la aplicación sea rápida, segura y fácil de ampliar en el futuro.

**¿Qué produce?** Un diagrama que muestra cómo se relaciona toda la información entre sí (similar a un organigrama pero para datos), y la estructura técnica lista para empezar a guardar información real.

**¿Por qué es importante?** Una base de datos mal diseñada es como construir una casa sobre cimientos débiles: funciona al principio, pero se vuelve un problema costoso a medida que crece.

---

### 3.3 Diseño del formulario completo de medición
**¿Qué es?** El formulario de medición es el corazón de la aplicación. Tiene múltiples secciones (información general, condiciones meteorológicas, fuentes de ruido, resultados, croquis, etc.) que el técnico llena durante y después de la visita de campo.

**¿Qué produce?** Un formulario digital de múltiples pasos, con validaciones automáticas que le avisan al técnico si algo está mal diligenciado, posibilidad de guardar el progreso y continuar después, y adaptado exactamente a los formatos oficiales **`24-001-A30_`** y **`FO-GA-17`** exigidos por la normativa.

**¿Qué se incluye?**
- Secciones 2 a 7 del formulario (la sección 1 ya está construida).
- Croquis interactivo donde el técnico dibuja el plano del lugar arrastrando elementos (mesas, puertas, fuentes de ruido, puntos de medición, etc.).
- Posibilidad de adjuntar fotos tomadas en campo.
- Guardado automático para no perder información si se cierra el navegador.

---

### 3.4 Módulo de dispositivos de medición
**¿Qué es?** Un inventario digital de todos los equipos que usa la empresa para medir: sonómetros, calibradores acústicos, etc.

**¿Qué puede hacer el usuario?**
- Registrar cada equipo con su marca, modelo y número de serie.
- Cargar el certificado de calibración de cada equipo.
- Ver cuándo vence la próxima calibración y recibir alertas antes de que ocurra.
- Asignar un equipo específico a cada medición para que quede registrado en el informe.

**¿Por qué es importante?** La normativa exige que cada informe indique qué equipo se usó y que ese equipo esté calibrado. Este módulo garantiza que esa información siempre esté disponible y al día.

---

### 3.5 Módulo de registro de empresas
**¿Qué es?** Un directorio de los clientes y contratantes para quienes se realizan las mediciones.

**¿Qué puede hacer el usuario?**
- Registrar cada empresa con NIT, razón social, representante legal y datos de contacto.
- Añadir los diferentes sitios o instalaciones de cada empresa donde se hacen mediciones (con dirección, coordenadas y tipo de zona según la Resolución 0627).
- Consultar el historial de informes por empresa.

---

### 3.6 Módulo de gestión de informes
**¿Qué es?** El repositorio central donde viven todos los informes de la empresa. Funciona como una carpeta digital organizada y consultable.

**¿Qué puede hacer el usuario?**
- Ver todos los informes con su estado: **Borrador** (en proceso), **Emitido** (terminado y enviado) o **Aprobado** (revisado y validado).
- Buscar informes por cliente, fecha, técnico responsable o número de contrato.
- Ver el detalle completo de cualquier informe y su historial de cambios.
- El técnico solo puede editar sus propios informes; el administrador puede ver y gestionar todos.

---

### 3.7 Lectura automática de archivos Excel del equipo de medición
**¿Qué es?** Los sonómetros modernos exportan sus datos en archivos Excel con un formato específico. En lugar de que el técnico copie esos datos a mano al sistema (con el riesgo de errores que eso implica), la aplicación lee el archivo directamente.

**¿Cómo funciona para el usuario?** El técnico sube el archivo Excel que genera el sonómetro (y el de meteorología), y la aplicación extrae automáticamente todos los datos de medición y los asocia al informe que está editando.

**¿Cuántos formatos se soportan?** Los dos templates específicos que usa la empresa actualmente. Si en el futuro se cambia de equipo o de formato, se puede ampliar.

---

### 3.8 Cálculo automático de indicadores acústicos
**¿Qué es?** Una vez que los datos del sonómetro están en el sistema, la aplicación hace todos los cálculos matemáticos que exige la normativa sin intervención humana.

**¿Qué calcula?**
- **Nivel equivalente (Leq):** el nivel de ruido promedio durante el período de medición.
- **Percentiles (L90, L50, L10):** indican cómo se distribuyó el ruido durante la medición.
- **Correcciones** por tipo de ruido (impulsivo, tonal).
- **Incertidumbre de medición:** margen de error técnico del resultado, exigido por la norma.

**¿Por qué es importante?** Elimina los errores de cálculo humano, que son la principal causa de informes rechazados o con observaciones por parte de las autoridades ambientales.

---

### 3.9 Módulo del profesional de medición
**¿Qué es?** Un registro de los profesionales habilitados para firmar los informes de medición (ingenieros, técnicos certificados, etc.).

**¿Qué puede hacer el usuario?**
- Registrar cada profesional con su nombre, número de tarjeta profesional y vigencia de sus certificaciones.
- Cargar la firma digitalizada del profesional.
- Asignar un profesional a cada informe para que su nombre y firma aparezcan automáticamente en el documento final generado.

---

### 3.10 Módulo de administración de usuarios
**¿Qué es?** El control de quién puede entrar a la aplicación y qué puede hacer dentro de ella.

**¿Qué roles existen?**
- **Administrador:** acceso total. Puede ver todos los informes, gestionar usuarios y configurar el sistema.
- **Técnico:** puede crear y editar sus propios informes y ver los equipos disponibles.
- **Cliente:** acceso de solo lectura a los informes que le corresponden. No puede editar nada.

---

### 3.11 Generación del documento final
**¿Qué es?** Al terminar de diligenciar el informe en la aplicación, el sistema genera automáticamente el documento oficial listo para entregar.

**¿Qué produce?**
- El archivo **Excel** con el formato exacto `24-001-A30_` entregado por el cliente, con todos los campos rellenados automáticamente a partir de lo que se digitó en el sistema.
- Un **PDF** listo para imprimir o enviar por correo electrónico, con la firma del profesional incluida.

**¿Por qué es importante?** Es el entregable final al cliente o a la autoridad ambiental. Que salga correcto, completo y con el formato exacto exigido es el objetivo principal de toda la aplicación.

---

### 3.12 Publicación de la aplicación en internet (servidor en la nube)
**¿Qué es?** Para que los técnicos puedan usar la aplicación desde cualquier lugar (oficina, campo, celular), necesita estar publicada en internet en un servidor. Este proceso se llama "despliegue".

**¿Qué incluye?**
- Configurar el servidor donde vivirá la aplicación.
- Asignar un dominio (ej. `ruido.miempresa.com.co`).
- Activar el candado de seguridad (HTTPS) para que la conexión sea cifrada.
- Configurar copias de seguridad automáticas de la base de datos.
- Documentar el proceso para que en el futuro cualquier técnico pueda actualizar la aplicación.

---

### 3.13 Administración del código fuente (GitHub)
**¿Qué es?** GitHub es la plataforma donde se guarda y versiona el código de la aplicación. Funciona como un historial completo de todos los cambios que se han hecho: quién hizo qué, cuándo y por qué. Si algo sale mal, se puede volver a una versión anterior en minutos.

**¿Qué incluye?**
- Organización del repositorio con buenas prácticas.
- Proceso automatizado para detectar errores antes de publicar cambios.
- Documentación técnica para que cualquier desarrollador que entre al proyecto en el futuro pueda entender el código rápidamente.

**¿Por qué le importa al cliente?** Al final del proyecto, el cliente recibe acceso completo al repositorio. El código es suyo.

---

### 3.14 Soporte posterior al lanzamiento (6 meses)
**¿Qué es?** Durante los 6 meses siguientes al lanzamiento oficial, el equipo está disponible para corregir errores que aparezcan en el uso real, hacer ajustes menores y garantizar que la aplicación funcione correctamente.

**¿Qué incluye?**
- Corrección de errores reportados por los usuarios (hasta 2 por mes).
- Actualización de dependencias de seguridad.
- Monitoreo básico de disponibilidad (que la app esté en línea).
- Ajustes menores de configuración.

**¿Qué no incluye?** Nuevas funcionalidades o cambios de alcance. Esos se cotizan por separado.

---

## 4. ¿Cómo se construye? — Plan de trabajo por etapas

El desarrollo se divide en 6 etapas. Al final de cada una se hace una revisión con el cliente antes de continuar.

---

### Etapa 1 — Entender el negocio (Semanas 1–2)
**¿Qué pasa en esta etapa?**
El equipo técnico se reúne con los responsables del proceso de medición para entender en detalle cómo trabajan, qué datos manejan, qué documentos producen y qué problemas tienen hoy con el proceso manual. Se revisan las plantillas Excel actuales y se documentan todos los requerimientos.

**¿Qué entrega al cliente?**
Un documento con el alcance detallado del proyecto, aprobado y firmado por ambas partes. Esto protege al cliente de cobros no esperados y al equipo de pedidos que no estaban acordados.

---

### Etapa 2 — Diseñar los cimientos (Semanas 3–4)
**¿Qué pasa en esta etapa?**
Se diseña la estructura de datos de la aplicación (cómo se organiza y relaciona toda la información) y se construyen los módulos base: el registro de empresas y el inventario de dispositivos de medición.

**¿Qué entrega al cliente?**
Los módulos de empresas y dispositivos funcionando, y el diagrama de la base de datos para revisión.

---

### Etapa 3 — El formulario completo (Semanas 5–8)
**¿Qué pasa en esta etapa?**
Se construyen todas las secciones del formulario de medición: condiciones del entorno, fuentes de ruido, croquis interactivo, condiciones meteorológicas, datos del receptor y resultados. Esta es la etapa más larga porque es el corazón de la aplicación.

**¿Qué entrega al cliente?**
El formulario completo funcionando, con guardado de borradores y validaciones. El cliente puede probarlo con datos reales.

---

### Etapa 4 — Los datos del equipo y los cálculos (Semanas 9–11)
**¿Qué pasa en esta etapa?**
Se construye el lector de archivos Excel del sonómetro y del informe meteorológico. También se programa el motor de cálculo que obtiene el Leq, los percentiles y la incertidumbre automáticamente a partir de los datos importados.

**¿Qué entrega al cliente?**
El técnico puede subir el Excel del equipo y ver los resultados calculados en pantalla, listos para ser incluidos en el informe.

---

### Etapa 5 — Gestión completa de informes y profesionales (Semanas 12–14)
**¿Qué pasa en esta etapa?**
Se construye el repositorio de informes (búsqueda, filtros, estados), el módulo del profesional de medición con su firma digitalizada, y el módulo de administración de usuarios con los tres roles definidos.

**¿Qué entrega al cliente?**
Un sistema completo donde los técnicos crean informes, un revisor los aprueba, y los clientes los consultan.

---

### Etapa 6 — Documento final y publicación (Semanas 15–18)
**¿Qué pasa en esta etapa?**
Se construye la exportación del informe en el Excel oficial y en PDF. Luego se publica la aplicación en el servidor elegido, se configura el dominio y la seguridad, y se hace una sesión de capacitación con los usuarios finales.

**¿Qué entrega al cliente?**
La aplicación publicada en internet, funcionando, con dominio propio y lista para uso productivo.

---

**Duración total: 18 semanas (aproximadamente 4 meses y medio)**

---

## 5. Estimación de Costos

### ¿Cómo se calculan los costos de desarrollo?

El desarrollo se cobra por **días efectivos de trabajo** a una tarifa de **$350.000 COP por día**. Un día efectivo equivale a 4–6 horas de trabajo enfocado de un desarrollador senior asistido por inteligencia artificial. Esta tarifa es entre un 50% y un 65% menor que la de un desarrollador senior contratado de forma tradicional en el mercado colombiano.

### 5.1 Desglose del desarrollo

| # | ¿Qué se construye? | Días | Costo COP |
|---|-------------------|:----:|----------:|
| 1 | Relevamiento de requerimientos (reuniones + documento de alcance) | 5 | $1.750.000 |
| 2 | Diseño de la base de datos (estructura y organización de toda la información) | 6 | $2.100.000 |
| 3 | Formulario completo de medición — secciones 2 a 7 con croquis interactivo | 14 | $4.900.000 |
| 4 | Formulario FO-GA-17 (informe de resultados complementario) | 5 | $1.750.000 |
| 5 | Módulo de dispositivos de medición (inventario + calibraciones + alertas) | 5 | $1.750.000 |
| 6 | Módulo de empresas y sitios de monitoreo | 4 | $1.400.000 |
| 7 | Módulo de gestión de informes (estados, repositorio, búsqueda y filtros) | 7 | $2.450.000 |
| 8 | Lectura automática de archivos Excel del sonómetro y de meteorología | 6 | $2.100.000 |
| 9 | Cálculo automático de indicadores acústicos (Leq, L90, L50, L10, incertidumbre) | 8 | $2.800.000 |
| 10 | Módulo de usuarios y roles (administrador, técnico, cliente) | 4 | $1.400.000 |
| 11 | Módulo del profesional de medición (registro, certificaciones, firma) | 4 | $1.400.000 |
| 12 | Generación del documento final en Excel oficial y PDF | 9 | $3.150.000 |
| 13 | Administración del repositorio GitHub (organización, automatizaciones, documentación) | 3 | $1.050.000 |
| 14 | Publicación en servidor (dominio, seguridad, copias de seguridad, capacitación) | 4 | $1.400.000 |
| | **Subtotal desarrollo** | **84 días** | **$29.400.000** |

---

### 5.2 Costos de servidor (12 meses)

La aplicación necesita un servidor en internet para funcionar. Es similar a pagar arriendo por el espacio donde vive la aplicación. Se presentan tres opciones con distintos precios y niveles de complejidad.

> Todos los precios de servidores están en dólares (USD) y se convierten a pesos a TRM $4.200. El costo real en pesos variará según la tasa de cambio del momento de pago.

---

#### Opción A — Amazon Web Services (AWS)
**¿Qué es?** El proveedor de nube más grande del mundo, usado por empresas como Netflix, Bancolombia y el gobierno de EE.UU. Ofrece la mayor cantidad de servicios y garantías.

**¿Cuándo elegirlo?** Cuando hay requisitos corporativos o contractuales que exigen un proveedor específico, o cuando se proyecta un crecimiento muy rápido de usuarios.

**¿Qué tan difícil es configurarlo?** Es el más complejo de los tres. Tiene muchos servicios separados que hay que conectar entre sí. No es recomendable para quien no tenga experiencia en AWS.

**Ventaja especial:** el primer año es casi gratis gracias al plan de capa gratuita (Free Tier) de AWS.

| ¿Qué se paga? | ¿Para qué sirve? | USD/mes | COP/año |
|--------------|-----------------|--------:|--------:|
| EC2 t3.small | El servidor donde corre la aplicación | ~$17 | $856.800 |
| RDS db.t3.micro | La base de datos gestionada por AWS | ~$20 | $1.008.000 |
| S3 Standard | Almacenamiento de archivos (informes, fotos, Excel) | ~$5 | $252.000 |
| Route 53 + dominio | La dirección web (ej. ruido.miempresa.com.co) | ~$2 | $130.400 |
| SSL (ACM) | El candado de seguridad HTTPS | Gratis | — |
| **Total Opción A** | | **~$44/mes** | **$2.247.200/año** |

> Año 1 con Free Tier: solo se paga almacenamiento + dominio → **~$382.400 COP**

---

#### Opción B — DigitalOcean
**¿Qué es?** Un proveedor de nube orientado a desarrolladores y empresas medianas. Más simple que AWS, con una interfaz muy intuitiva y documentación excelente en español.

**¿Cuándo elegirlo?** Cuando se quiere un balance entre simplicidad y funcionalidades. Es la opción más fácil de administrar para alguien sin perfil técnico.

**¿Qué tan difícil es configurarlo?** Nivel medio. Tiene un panel de control muy visual y hasta plantillas preconfiguradas para aplicaciones como la nuestra.

| ¿Qué se paga? | ¿Para qué sirve? | USD/mes | COP/año |
|--------------|-----------------|--------:|--------:|
| Droplet Basic | El servidor de la aplicación | ~$18 | $907.200 |
| Managed MySQL | Base de datos gestionada por DigitalOcean | ~$15 | $756.000 |
| Spaces | Almacenamiento de archivos | ~$5 | $252.000 |
| Dominio | La dirección web | — | $130.000 |
| SSL | Gratuito con Let's Encrypt | Gratis | — |
| **Total Opción B** | | **~$38/mes** | **$2.045.200/año** |

---

#### Opción C — Hetzner
**¿Qué es?** Un proveedor alemán con servidores también en EE.UU. Es el más económico de los tres por amplio margen, muy popular entre startups y proyectos que quieren maximizar el presupuesto.

**¿Cuándo elegirlo?** Cuando el presupuesto de infraestructura es una prioridad y no hay requisitos corporativos de usar AWS o DigitalOcean.

**¿Qué tan difícil es configurarlo?** Nivel medio-bajo. Es un servidor básico sin servicios extra: la base de datos vive en el mismo servidor que la aplicación, lo que es perfectamente suficiente para el volumen de usuarios esperado.

| ¿Qué se paga? | ¿Para qué sirve? | USD/mes | COP/año |
|--------------|-----------------|--------:|--------:|
| VPS CX21 | Servidor con 4 GB RAM (aplicación + base de datos juntas) | ~$6 | $302.400 |
| Object Storage | Almacenamiento de archivos | ~$6 | $302.400 |
| Dominio | La dirección web | — | $130.000 |
| SSL | Gratuito con Let's Encrypt | Gratis | — |
| **Total Opción C** | | **~$12/mes** | **$734.800/año** |

---

#### ¿Cuál elegir? — Comparativo rápido

| | AWS | DigitalOcean | Hetzner |
|-|:---:|:---:|:---:|
| Costo anual | $2.247.200 | $2.045.200 | $734.800 |
| Dificultad de administración | Alta | Media | Media-baja |
| Facilidad de uso del panel | ★★☆ | ★★★ | ★★☆ |
| Confiabilidad / garantías | ★★★ | ★★★ | ★★☆ |
| Soporte en español | ★★☆ | ★★★ | ★★☆ |
| Servidor en Latinoamérica | Sí (São Paulo) | No | No (EE.UU.) |

> **Recomendación para este proyecto:** empezar con **Hetzner (Opción C)** para reducir costos en la etapa inicial. Si en el futuro crece el número de usuarios o hay exigencias contractuales, migrar a AWS es un proceso manejable. Si se elige AWS, aprovechar el Free Tier el primer año reduce el costo de infraestructura a menos de $400.000 COP.

---

### 5.3 Soporte posterior al lanzamiento (6 meses)

Una vez publicada la aplicación, el equipo permanece disponible para atender problemas y hacer ajustes menores durante los primeros 6 meses. Esto garantiza una transición tranquila del proceso manual al digital.

| ¿Qué cubre? | Costo COP |
|------------|----------:|
| Corrección de errores de funcionamiento (hasta 2 reportes por mes) | $1.500.000 |
| Actualizaciones de seguridad y mantenimiento preventivo | $900.000 |
| Ajustes menores de configuración y consultas técnicas | $600.000 |
| **Subtotal soporte 6 meses** | **$3.000.000** |

> Pasados los 6 meses, si el cliente desea continuar con soporte, se firma un contrato de mantenimiento por separado.

---

### 5.4 Resumen de la inversión total

| Rubro | AWS (A) | DigitalOcean (B) | Hetzner (C) |
|-------|--------:|----------------:|------------:|
| Desarrollo de software | $29.400.000 | $29.400.000 | $29.400.000 |
| Servidor en la nube — 12 meses | $2.247.200 | $2.045.200 | $734.800 |
| Soporte post-lanzamiento — 6 meses | $3.000.000 | $3.000.000 | $3.000.000 |
| **TOTAL ANTES DE IMPUESTOS** | **$34.647.200** | **$34.445.200** | **$33.134.800** |
| Con IVA (19%) | ~$41.230.000 | ~$40.990.000 | ~$39.430.000 |

> Con **AWS Free Tier** el primer año, el total con opción A baja a aproximadamente **$32.782.400 COP** (antes de IVA).

---

## 6. Condiciones, acuerdos y lo que no está incluido

### Lo que el cliente debe proveer

- Acceso oportuno a las plantillas Excel del sonómetro y del informe meteorológico.
- Un técnico de su equipo disponible para validar que los cálculos automáticos coincidan con los manuales actuales.
- Respuesta a las revisiones de cada etapa dentro de los **10 días hábiles** siguientes a la entrega. Si no hay respuesta, se entiende que la etapa está aprobada.

### Lo que NO está incluido en este presupuesto

- **Aplicación móvil nativa** (para iPhone o Android como app descargable). Lo que se construye es una aplicación web que funciona en el navegador del celular, que cubre el 95% de los casos de uso de campo.
- **Firma electrónica con validez jurídica** (como las que usan notarías o la DIAN). La firma del profesional es una imagen digitalizada, suficiente para el proceso de revisión interna. Si se requiere firma con validez legal, es un módulo adicional.
- **Integración con otros sistemas** del cliente (ERP, sistemas de contabilidad, plataformas gubernamentales). Si se necesita, se cotiza por separado.
- **Capacitaciones adicionales** más allá de la sesión de lanzamiento incluida. Si se requieren talleres adicionales, se cotizan por separado.

### Forma de pago sugerida

| Momento | Porcentaje | Monto aprox. (Hetzner, sin IVA) |
|---------|:----------:|--------------------------------:|
| Al firmar el contrato | 30% | ~$9.940.000 |
| Al entregar la Etapa 4 (cálculos funcionando) | 40% | ~$13.254.000 |
| Al lanzamiento y cierre del proyecto | 30% | ~$9.940.000 |

- **Validez de esta propuesta:** 30 días calendario desde la fecha de emisión.
- **Moneda:** Pesos colombianos (COP). Los costos de servidor en USD se liquidan a la TRM del día de pago.

---

## 7. ¿Por qué el desarrollo con IA es confiable?

Es normal tener dudas sobre usar inteligencia artificial para construir software. Estas son las preguntas más frecuentes:

**¿La IA escribe código de mala calidad?**
No, cuando está dirigida por un experto. Claude Code es una herramienta: el desarrollador senior decide qué construir, revisa todo el código generado y corrige lo que no cumple con los estándares. La IA acelera el trabajo de digitación y exploración de soluciones; el criterio técnico siempre es humano.

**¿Qué pasa si el desarrollador se enferma o no sigue disponible?**
El código queda documentado, en un repositorio de GitHub que es propiedad del cliente, con estándares que cualquier otro desarrollador puede entender. No hay dependencia de una sola persona que guarda el conocimiento en su cabeza.

**¿Es más barato porque es de menor calidad?**
No. Es más barato porque la productividad es mayor. Construir la misma funcionalidad le toma menos tiempo, y ese ahorro de tiempo se traslada directamente al precio.

---

*Documento preparado por el equipo técnico del proyecto · Abril 2026*
*Para consultas o ajustes de alcance, contactar al responsable técnico.*
