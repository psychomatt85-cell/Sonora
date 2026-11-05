# Sonora
agente de recomendación musical
A U R A
Automatización de recomendaciones musicales
Documentación funcional, técnica y económica
n8n · Telegram · Agentes de IA · RapidAPI / TheAudioDB · Google Sheets


1. Resumen ejecutivo
Aura es un asistente conversacional de descubrimiento musical. Recibe mensajes por Telegram, interpreta si el usuario solicita música, selecciona artista y álbum, consulta información externa y devuelve una respuesta en lenguaje natural.
La solución se orquesta mediante n8n y combina agentes de IA, RapidAPI con TheAudioDB y Google Sheets. El objetivo es reducir tareas manuales de selección, búsqueda y redacción, mantener trazabilidad y permitir escalar el servicio.
En una etapa inicial, las recomendaciones se orientan al descubrimiento musical general. A futuro, podría evolucionar hacia recomendaciones asociadas a catálogos o acuerdos con discográficas.
2. Contexto empresarial y necesidad
Aura se plantea como una startup pequeña que brinda recomendaciones musicales. En un esquema manual, cada consulta requiere interpretar el pedido, seleccionar un artista y álbum, buscar datos complementarios y redactar la respuesta. El crecimiento del volumen puede incrementar la carga operativa y dificultar la consistencia.
La automatización permite atender consultas de forma continua y estandarizar el proceso. Además, abre la posibilidad de incorporar reglas comerciales, como priorizar lanzamientos o artistas de sellos discográficos asociados.
3. Objetivos y alcance funcional
Objetivo general
Automatizar el proceso de recomendación musical desde Telegram: interpretación, selección, enriquecimiento de información y respuesta.
Objetivos específicos
Distinguir solicitudes musicales de mensajes conversacionales generales.
Seleccionar artista y álbum adecuados a la consulta.
Consultar información musical complementaria mediante TheAudioDB.
Redactar una respuesta cercana, en español latinoamericano y adecuada para Telegram.
Registrar información operativa, recomendaciones e incidencias en Google Sheets.
Contemplar una respuesta alternativa ante fallos técnicos.
No se verifica en esta versión un catálogo propio, recomendación contractual con discográficas, analítica avanzada o una base transaccional dedicada. Se consideran evoluciones futuras.
4. Flujo funcional
Recepción: El Trigger de Telegram recibe el mensaje.
Captura de contexto: Captura info extrae Chat ID, nombre, username, fecha/hora y texto.
Interpretación y selección: sonora clasifica el mensaje y selecciona artista, álbum y contexto; su salida esperada es JSON.
Preparación: Un nodo JavaScript parsea el resultado y expone los campos.
Enriquecimiento: sonora1 consulta la API musical y prepara la respuesta conversacional.
Enrutamiento: Organiza respuesta y Enruta respuesta separan conversación general, recomendación y error.
Registro: Las ramas de recomendación y error disponen de herramientas de Google Sheets.
Entrega: El nodo de Telegram envía la respuesta al chat de origen.
5. Arquitectura y componentes
Nodo / componente
Función documentada
escucha telegram
Trigger para nuevos mensajes de Telegram.
Captura info
Captura identificador de chat, nombre, username, fecha/hora y mensaje.
sonora
Agente que clasifica intención y selecciona artista/álbum.
Code in JavaScript
Parsea el JSON de salida de sonora.
sonora1
Agente que consulta datos musicales y genera respuesta.
API Rapid
Herramienta HTTP para consultar la API musical.
Organiza respuesta / Enruta respuesta
Normalización y derivación de resultados.
actua frente al error
Agente de contingencia y comunicación con el usuario.
registra respuesta
Agente conectado a Google Sheets para persistencia.
envia la respuesta
Envía texto a Telegram.


6. Herramientas conectadas
6.1 Telegram
Canal de entrada y salida. Permite recibir mensajes y responder en el mismo chat. Se capturan Chat ID, nombre, username, fecha/hora y texto.
6.2 n8n Cloud
Plataforma de orquestación de nodos, agentes, APIs y lógica condicional. Cloud evita administrar servidores propios, sistema operativo, actualizaciones de infraestructura y disponibilidad del hosting. El costo depende del plan y de las ejecuciones.
No se confirmó si el abono de n8n Cloud está incluido en los USD 300 de mantenimiento; se considera un costo a validar para evitar doble contabilización.
6.3 Agentes y modelos
El JSON muestra agentes principales y de persistencia conectados a modelos Ollama identificados como Beam. El agente de contingencia utiliza un modelo MiniMax identificado como MiniMax-M3..
6.4 RapidAPI / TheAudioDB
RapidAPI funciona como puerta de acceso a TheAudioDB. La consulta prevista utiliza artista (parámetro s) y álbum (parámetro a) para recuperar detalles musicales y enriquecer la recomendación.
6.5 Google Sheets
Google Sheets se utiliza como base auxiliar para logs de errores, recomendaciones/interacciones y contactos. Es una alternativa simple y de bajo costo para una primera etapa, con revisión manual sencilla.
El proyecto incluye una herramienta de append para errores y otra herramienta conectada al agente de persistencia. 
7. Entradas, procesamiento y salidas
Etapa
Información
Entrada
Mensaje de texto por Telegram.
Contexto capturado
Chat ID, nombre, username, fecha/hora y texto.
Salida de sonora
category, artist, album y context_note.
Consulta API
Artista y álbum como parámetros de búsqueda.
Salida final
Texto conversacional listo para Telegram.
Persistencia
Logs de errores y recomendaciones; contactos sujetos a completar el mapeo.


8. Gestión de errores y trazabilidad
El workflow contempla una rama de error que deriva a un agente de IA para redactar un mensaje breve y tranquilizador al usuario, evitando exponer detalles técnicos.
La herramienta «Listado de errores» está configurada para agregar una fila en Google Sheets con fecha y texto asociado a la salida del agente sonora. La conexión de una herramienta a un agente no garantiza que se ejecute en todos los casos; se recomienda asegurar el registro con una instrucción obligatoria o un nodo determinístico.
La propuesta de mantenimiento contempla hasta cinco intervenciones correctivas por mes. A partir de la sexta, se cobra USD 50 por intervención adicional.
9. Análisis económico y ROI (escenario proyectado)
Los volúmenes y tiempos siguientes son supuestos ilustrativos para estimar capacidad y ahorro. Deben reemplazarse por métricas reales al contar con datos de operación.
Variable
Supuesto
Consultas mensuales
3.000
Tiempo manual promedio por consulta
5 minutos
Carga manual equivalente
250 horas mensuales
Jornada de referencia
160 horas mensuales
Salario de referencia
ARS 1.000.000 por persona/mes
Costo salarial proporcional estimado
ARS 1.562.500/mes (250 / 160 × 1.000.000)


El costo salarial proporcional no incluye cargas sociales, beneficios, supervisión, licencias ni coordinación. Las horas liberadas representan capacidad que podría reasignarse; no necesariamente eliminación de puestos.
9.1 Inversión y mantenimiento
Concepto
Costo / condición
Implementación inicial
USD 150, pago único; mano de obra inicial.
Mantenimiento
USD 300 mensuales.
Incluido en mantenimiento
LLM y módulo de memoria más robusto, según propuesta.
Soporte correctivo
Hasta 5 intervenciones mensuales incluidas.
Intervención adicional
USD 50 cada una, desde la sexta.
n8n Cloud
Plan/costo a confirmar; inclusión en mantenimiento no verificada.


Primer mes: USD 450 por implementación y mantenimiento, más n8n Cloud si se factura por separado. Meses siguientes: USD 300 mensuales, más costos no incluidos y eventuales intervenciones adicionales.
9.2 Punto de equilibrio y ahorro
Costo manual estimado: ARS 1.562.500/mes. Para comparar con USD 300, el punto de equilibrio del mantenimiento corresponde a un tipo de cambio de ARS 5.208,33/USD (sin n8n Cloud ni costos variables).
Ahorro neto mensual = costo laboral manual evitado − mantenimiento − suscripciones no incluidas − costos variables. En el primer mes debe descontarse además la implementación. No se fija un ROI porcentual definitivo sin validar tipo de cambio, volumen real y costos Cloud.
La proyección no representa ahorro garantizado; depende de que el trabajo manual se reduzca o pueda reasignarse efectivamente.
10. KPIs propuestos
Indicador
Definición
Consultas automatizadas por mes
Cantidad de mensajes procesados; mide volumen y adopción.
Tasa de error
Errores registrados / consultas procesadas × 100; mide estabilidad técnica.


11. Justificación de la automatización y de la IA
La automatización convierte una tarea repetitiva en un servicio continuo, reduce la dependencia de intervención manual por consulta y estandariza el proceso. También permite aumentar el volumen y mantener registros operativos.
La IA se justifica frente a un chatbot tradicional porque los pedidos musicales se expresan en lenguaje natural y pueden basarse en artistas, géneros, estados de ánimo, actividades o referencias similares. Un bot rígido necesitaría numerosas opciones y rutas predefinidas. La IA interpreta pedidos abiertos y genera respuestas contextualizadas, mientras n8n controla integraciones y enrutamiento.
12. Escalabilidad y evolución
Modelos de IA
La arquitectura permite sustituir o complementar modelos actuales por modelos más capaces, como modelos de OpenAI accesibles mediante API. Esto puede mejorar la comprensión de pedidos ambiguos, consistencia y redacción. El uso de API se factura por tokens y es independiente de una suscripción personal de ChatGPT. Los precios deben verificarse al momento de contratar.
Memoria y almacenamiento
El JSON actual utiliza Simple Memory con ventanas de contexto. La propuesta de mantenimiento contempla una memoria más robusta. Google Sheets puede evolucionar a una base como PostgreSQL para usuarios, preferencias, recomendaciones, auditoría y métricas.
Infraestructura
A futuro, Aura puede migrar de n8n Cloud a infraestructura propia o cloud administrada. Esto exige evaluar costos de operación, seguridad, backups, monitoreo, actualizaciones y disponibilidad.
Evolución comercial
Se podrían incorporar acuerdos con discográficas y reglas para priorizar artistas o lanzamientos asociados. Las recomendaciones promocionadas deberían distinguirse de las recomendaciones editoriales independientes.
13. Anexos visuales y técnicos
Anexo A – Ejecución de workflow

Captura de n8n Executions: ejecución reportada como Succeeded y evento de Telegram recibido.
Anexo B – Vista general del workflow

Captura de la arquitectura de nodos, agentes, API, enrutamiento, registro y respuesta.
Anexo C – Prompts

# IDIOMA Y TONO — OBLIGATORIO

Debes responder SIEMPRE en **español latinoamericano**. 
- Usa un lenguaje natural, relajado, moderno y amigable.
- Utiliza expresiones idiomáticas latinoamericanas de forma orgánica (por ejemplo: *"¡Uff, discazo!", "Está tremendo", "Ese tema es un viaje", "Ojo con este álbum"*, etc.).

# USO DEL NOMBRE DEL USUARIO

El nombre del usuario te llega en los datos de entrada en el campo:

{{ $json.message.chat.first_name }}

- **Regla de oro:** Llama al usuario por su nombre de forma natural (ej: *"¡Hola, {{ $json.message.chat.first_name }}! ¿Cómo va?"*, *"Escuchate este tema, {{ $json.message.chat.first_name }}..."*).
- Si el campo `first_name` viene vacío, no está disponible o es un usuario anónimo, usa un trato amigable sin inventar un nombre.

# ROL Y PERSONALIDAD

Sos **Aura**, un/a apasionado/a, melómano/a y gran compañero/a musical.

Tu misión es recibir una recomendación musical en bruto (artista, álbum y nota de contexto), buscar más detalles técnicos o curiosidades en la API conectada, y redactar un mensaje increíble para enviar por Telegram.

Tu personalidad:
- **Cercano/a y conversacional:** Escribís como un verdadero amante de la música hablándole a un amigo por un chat. Nada de formalidades corporativas ni tono de robot.
- **Melómano/a y culto/a:** Tenés "barrio musical", sabés contar anécdotas, describir la atmósfera del sonido (pesado, denso, espacial, groove, lo-fi, etc.) y enganchar al oyente.
- **Curioso/a:** Siempre cerrás tu recomendación con una pregunta para seguir la charla.

# DATOS DE ENTRADA Y USO DE LA TOOL (API HTTP)

Recibes la sugerencia previa con estos datos:
- **Artista:** {{ $json.artist }}
- **Álbum:** {{ $json.album }}
- **Nota de Contexto:** {{ $json.context_note }}

### REGLAS DE EJECUCIÓN DE LA TOOL:
1. **Si `artist` y `album` NO son null:**
   - DEBES llamar a la Tool de búsqueda HTTP pasando OBLIGATORIAMENTE los query parameters:
     * `s`: "{{ $json.artist }}"
     * `a`: "{{ $json.album }}"
   - Usa la información devuelta por la API (año de lanzamiento, género, tracks destacados, sello, datos curiosos) para enriquecer y ponerle "condimento" a la respuesta.
2. **Si `artist` y `album` son null (Chat sin recomendación):**
   - NO llames a la Tool. Simplemente redactá una respuesta conversacional amigable para continuar el chat o saludar al usuario.
3. **Ejecución silenciosa:** NUNCA le menciones al usuario que buscaste datos en una API o ejecutaste una herramienta. Incorporá la información de forma 100% natural.

# TAREAS DE REDACCIÓN (EL "CONDIMENTO")

Cuando recomiendes un disco:
- **Mencioná el artista y el álbum de forma clara.**
- **Agregá el condimento musical:** Usá los datos de la API y la `context_note` para explicar *por qué* vale la pena escucharlo, qué vibra transmite, o algún dato de la producción o la banda que sume color.
- **Formato celular:** Párrafos cortos (1 a 3 líneas máx), emojis con buen gusto (🎶, 🎧, 🎸, ☕, ✨) y sin bloques de código Markdown complejos.
- **Plataformas:** Mencioná de forma natural que lo puede buscar en Spotify, Apple Music o YouTube Music.

# REGLA DE SALIDA (ESTRICTO)

Devuelve **únicamente** la respuesta conversacional final redactada por Aura, lista para enviar al nodo de Telegram.

No incluyas JSON, metadatos, ni explicaciones internas.

Conclusión
Aura presenta una arquitectura modular para automatizar recomendaciones musicales por Telegram. n8n coordina los agentes, las consultas externas y la persistencia auxiliar. La solución permite una primera etapa de bajo esfuerzo de infraestructura y deja abierta la evolución hacia modelos más capaces, memoria persistente y bases de datos robustas. Antes de producción, deben validarse los contratos de salida, el endpoint de TheAudioDB y los mecanismos de persistencia.
