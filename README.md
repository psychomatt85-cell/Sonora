# Sonora
1. Descripción general
1.1. Objetivo del sistema
Aura, identificada como SONORA dentro del workflow de n8n, es un asistente musical conversacional diseñado para interactuar con usuarios mediante Telegram.

Su objetivo principal es interpretar los mensajes recibidos y determinar si el usuario necesita una recomendación musical. Cuando corresponde, selecciona un artista y un álbum concreto, consulta una herramienta externa para obtener información complementaria y redacta una respuesta personalizada.

El sistema también contempla:
- Conversaciones generales sin recomendación musical.
- Personalización de las recomendaciones a partir del contexto conversacional.
- Incorporación de documentos PDF sobre historia de la música a una base vectorial.
- Registro de las respuestas y recomendaciones en Google Sheets.
- Registro de errores y generación de respuestas alternativas.
- Un segundo canal de entrada mediante Gmail, incorporado en esta versión del workflow.
1.2. Alcance funcional
La arquitectura utiliza dos agentes principales:
1. SONORA: interpreta el mensaje, clasifica la intención y selecciona un artista y un álbum.
2. SONORA1: recibe la selección, consulta la herramienta HTTP cuando corresponde y redacta el mensaje final.
Se incorporan, además, dos agentes auxiliares:
- actua frente al error: genera un mensaje alternativo ante un inconveniente.
- registra respuesta: gestiona el registro de las interacciones en Google Sheets.
La solución se complementa con integraciones de Telegram, Gmail, Google Drive, Supabase, Google Gemini, Groq, MiniMax, Ollama y Google Sheets.

2. Arquitectura general
2.1. Componentes
Componente	Responsabilidad
Telegram Trigger	Recibir mensajes de usuarios.
Gmail Trigger	Recibir eventos de correo electrónico.
Captura info	Normalizar los datos de entrada.
Google Drive	Buscar y descargar documentos PDF.
Supabase Vector Store	Incorporar documentos a una base vectorial.
SONORA	Interpretar intención y seleccionar música.
Code in JavaScript	Procesar el JSON generado por SONORA.
SONORA1	Enriquecer y redactar la respuesta final.
API Rapid	Consultar información musical mediante HTTP.
Organiza respuesta	Interpretar y estructurar la salida de SONORA1.
Enruta respuesta	Derivar la ejecución según la categoría.
Agentes auxiliares	Gestionar errores y registros.
Google Sheets	Registrar información de las interacciones.
Telegram	Enviar la respuesta al usuario.

2.2. Diagrama del flujo principal
#chatgpt-mermaid-_r_1po_{font-family:-apple-system-body,ui-sans-serif,-apple-system,system-ui,"Segoe UI",Helvetica,"Apple Color Emoji",Arial,sans-serif,"Segoe UI Emoji","Segoe UI Symbol";font-size:16px;fill:rgb(237, 237, 237);}@keyframes edge-animation-frame{from{stroke-dashoffset:0;}}@keyframes dash{to{stroke-dashoffset:0;}}#chatgpt-mermaid-_r_1po_ .edge-animation-slow{stroke-dasharray:9,5!important;stroke-dashoffset:900;animation:dash 50s linear infinite;stroke-linecap:round;}#chatgpt-mermaid-_r_1po_ .edge-animation-fast{stroke-dasharray:9,5!important;stroke-dashoffset:900;animation:dash 20s linear infinite;stroke-linecap:round;}#chatgpt-mermaid-_r_1po_ .error-icon{fill:rgb(48, 48, 48);}#chatgpt-mermaid-_r_1po_ .error-text{fill:rgb(237, 237, 237);stroke:rgb(237, 237, 237);}#chatgpt-mermaid-_r_1po_ .edge-thickness-normal{stroke-width:1px;}#chatgpt-mermaid-_r_1po_ .edge-thickness-thick{stroke-width:3.5px;}#chatgpt-mermaid-_r_1po_ .edge-pattern-solid{stroke-dasharray:0;}#chatgpt-mermaid-_r_1po_ .edge-thickness-invisible{stroke-width:0;fill:none;}#chatgpt-mermaid-_r_1po_ .edge-pattern-dashed{stroke-dasharray:3;}#chatgpt-mermaid-_r_1po_ .edge-pattern-dotted{stroke-dasharray:2;}#chatgpt-mermaid-_r_1po_ .marker{fill:rgb(175, 175, 175);stroke:rgb(175, 175, 175);}#chatgpt-mermaid-_r_1po_ .marker.cross{stroke:rgb(175, 175, 175);}#chatgpt-mermaid-_r_1po_ svg{font-family:-apple-system-body,ui-sans-serif,-apple-system,system-ui,"Segoe UI",Helvetica,"Apple Color Emoji",Arial,sans-serif,"Segoe UI Emoji","Segoe UI Symbol";font-size:16px;}#chatgpt-mermaid-_r_1po_ p{margin:0;}#chatgpt-mermaid-_r_1po_ .label{font-family:-apple-system-body,ui-sans-serif,-apple-system,system-ui,"Segoe UI",Helvetica,"Apple Color Emoji",Arial,sans-serif,"Segoe UI Emoji","Segoe UI Symbol";color:rgb(237, 237, 237);}#chatgpt-mermaid-_r_1po_ .cluster-label text{fill:rgb(237, 237, 237);}#chatgpt-mermaid-_r_1po_ .cluster-label span{color:rgb(237, 237, 237);}#chatgpt-mermaid-_r_1po_ .cluster-label span p{background-color:transparent;}#chatgpt-mermaid-_r_1po_ .label text,#chatgpt-mermaid-_r_1po_ span{fill:rgb(237, 237, 237);color:rgb(237, 237, 237);}#chatgpt-mermaid-_r_1po_ .node rect,#chatgpt-mermaid-_r_1po_ .node circle,#chatgpt-mermaid-_r_1po_ .node ellipse,#chatgpt-mermaid-_r_1po_ .node polygon,#chatgpt-mermaid-_r_1po_ .node path{fill:rgb(9, 23, 44);stroke:rgb(31, 78, 148);stroke-width:1px;}#chatgpt-mermaid-_r_1po_ .rough-node .label text,#chatgpt-mermaid-_r_1po_ .node .label text,#chatgpt-mermaid-_r_1po_ .image-shape .label,#chatgpt-mermaid-_r_1po_ .icon-shape .label{text-anchor:middle;}#chatgpt-mermaid-_r_1po_ .node .katex path{fill:#000;stroke:#000;stroke-width:1px;}#chatgpt-mermaid-_r_1po_ .rough-node .label,#chatgpt-mermaid-_r_1po_ .node .label,#chatgpt-mermaid-_r_1po_ .image-shape .label,#chatgpt-mermaid-_r_1po_ .icon-shape .label{text-align:center;}#chatgpt-mermaid-_r_1po_ .node.clickable{cursor:pointer;}#chatgpt-mermaid-_r_1po_ .root .anchor path{fill:rgb(175, 175, 175)!important;stroke-width:0;stroke:rgb(175, 175, 175);}#chatgpt-mermaid-_r_1po_ .arrowheadPath{fill:rgb(175, 175, 175);}#chatgpt-mermaid-_r_1po_ .edgePath .path{stroke:rgb(175, 175, 175);stroke-width:1px;}#chatgpt-mermaid-_r_1po_ .flowchart-link{stroke:rgb(175, 175, 175);fill:none;}#chatgpt-mermaid-_r_1po_ .edgeLabel{background-color:rgb(0, 0, 0);text-align:center;}#chatgpt-mermaid-_r_1po_ .edgeLabel p{background-color:rgb(0, 0, 0);}#chatgpt-mermaid-_r_1po_ .edgeLabel rect{opacity:0.5;background-color:rgb(0, 0, 0);fill:rgb(0, 0, 0);}#chatgpt-mermaid-_r_1po_ .labelBkg{background-color:rgba(0, 0, 0, 0.5);}#chatgpt-mermaid-_r_1po_ .cluster rect{fill:rgb(48, 48, 48);stroke:rgba(255, 255, 255, 0.15);stroke-width:1px;}#chatgpt-mermaid-_r_1po_ .cluster text{fill:rgb(237, 237, 237);}#chatgpt-mermaid-_r_1po_ .cluster span{color:rgb(237, 237, 237);}#chatgpt-mermaid-_r_1po_ div.mermaidTooltip{position:absolute;text-align:center;max-width:200px;padding:2px;font-family:-apple-system-body,ui-sans-serif,-apple-system,system-ui,"Segoe UI",Helvetica,"Apple Color Emoji",Arial,sans-serif,"Segoe UI Emoji","Segoe UI Symbol";font-size:12px;background:rgb(48, 48, 48);border:1px solid rgba(255, 255, 255, 0.15);border-radius:2px;pointer-events:none;z-index:100;}#chatgpt-mermaid-_r_1po_ .flowchartTitleText{text-anchor:middle;font-size:18px;fill:rgb(237, 237, 237);}#chatgpt-mermaid-_r_1po_ rect.text{fill:none;stroke-width:0;}#chatgpt-mermaid-_r_1po_ .icon-shape,#chatgpt-mermaid-_r_1po_ .image-shape{background-color:rgb(0, 0, 0);text-align:center;}#chatgpt-mermaid-_r_1po_ .icon-shape p,#chatgpt-mermaid-_r_1po_ .image-shape p{background-color:rgb(0, 0, 0);padding:2px;}#chatgpt-mermaid-_r_1po_ .icon-shape .label rect,#chatgpt-mermaid-_r_1po_ .image-shape .label rect{opacity:0.5;background-color:rgb(0, 0, 0);fill:rgb(0, 0, 0);}#chatgpt-mermaid-_r_1po_ .label-icon{display:inline-block;height:1em;overflow:visible;vertical-align:-0.125em;}#chatgpt-mermaid-_r_1po_ .node .label-icon path{fill:currentColor;stroke:revert;stroke-width:revert;}#chatgpt-mermaid-_r_1po_ .node .neo-node{stroke:rgb(31, 78, 148);}#chatgpt-mermaid-_r_1po_ [data-look="neo"].node rect,#chatgpt-mermaid-_r_1po_ [data-look="neo"].cluster rect,#chatgpt-mermaid-_r_1po_ [data-look="neo"].node polygon{stroke:url(#chatgpt-mermaid-_r_1po_-gradient);filter:drop-shadow( 1px 2px 2px rgba(185,185,185,1));}#chatgpt-mermaid-_r_1po_ [data-look="neo"].swimlane.cluster rect{filter:none;}#chatgpt-mermaid-_r_1po_ [data-look="neo"].node path{stroke:url(#chatgpt-mermaid-_r_1po_-gradient);stroke-width:1px;}#chatgpt-mermaid-_r_1po_ [data-look="neo"].node .outer-path{filter:drop-shadow( 1px 2px 2px rgba(185,185,185,1));}#chatgpt-mermaid-_r_1po_ [data-look="neo"].node .neo-line path{stroke:rgb(31, 78, 148);filter:none;}#chatgpt-mermaid-_r_1po_ [data-look="neo"].node circle{stroke:url(#chatgpt-mermaid-_r_1po_-gradient);filter:drop-shadow( 1px 2px 2px rgba(185,185,185,1));}#chatgpt-mermaid-_r_1po_ [data-look="neo"].node circle .state-start{fill:#000000;}#chatgpt-mermaid-_r_1po_ [data-look="neo"].icon-shape .icon{fill:url(#chatgpt-mermaid-_r_1po_-gradient);filter:drop-shadow( 1px 2px 2px rgba(185,185,185,1));}#chatgpt-mermaid-_r_1po_ [data-look="neo"].icon-shape .icon-neo path{stroke:url(#chatgpt-mermaid-_r_1po_-gradient);filter:drop-shadow( 1px 2px 2px rgba(185,185,185,1));}#chatgpt-mermaid-_r_1po_ .node text{font-size:14px;font-weight:600;letter-spacing:normal;fill:rgb(153, 206, 255);}#chatgpt-mermaid-_r_1po_ .edgeLabels text{font-size:13px;font-weight:600;letter-spacing:-0.08px;fill:rgb(153, 206, 255);}#chatgpt-mermaid-_r_1po_ .node tspan[font-weight="normal"],#chatgpt-mermaid-_r_1po_ .edgeLabels tspan[font-weight="normal"]{font-weight:600;}#chatgpt-mermaid-_r_1po_ .edgeLabel .label rect{opacity:1;rx:13px;ry:13px;fill:rgb(0, 14, 26);stroke:rgb(26, 62, 95);stroke-width:1px;}#chatgpt-mermaid-_r_1po_ .node rect,#chatgpt-mermaid-_r_1po_ .node circle,#chatgpt-mermaid-_r_1po_ .node ellipse,#chatgpt-mermaid-_r_1po_ .node polygon,#chatgpt-mermaid-_r_1po_ .node path{fill:rgb(0, 40, 77);stroke:rgba(255, 255, 255, 0.1);stroke-width:1px;}#chatgpt-mermaid-_r_1po_ .node rect{rx:16px;ry:16px;}#chatgpt-mermaid-_r_1po_ .mermaid-decision .label-container{fill:rgb(0, 14, 26);stroke:rgb(26, 62, 95);stroke-dasharray:2,2;}#chatgpt-mermaid-_r_1po_ .edgePaths .flowchart-link{stroke:rgb(175, 175, 175);stroke-width:1px;stroke-linecap:round;stroke-linejoin:round;}#chatgpt-mermaid-_r_1po_ .marker{fill:rgb(175, 175, 175);stroke:rgb(175, 175, 175);}#chatgpt-mermaid-_r_1po_ :root{--mermaid-font-family:-apple-system-body,ui-sans-serif,-apple-system,system-ui,"Segoe UI",Helvetica,"Apple Color Emoji",Arial,sans-serif,"Segoe UI Emoji","Segoe UI Symbol";}Telegram TriggerCaptura infoGmail TriggerSearch files and foldersDownload fileSupabase Vector StoreSONORACode in JavaScriptSONORA1Organiza respuestaEnruta respuestaorganiza datos respuestaactua frente al errorregistra respuestaenvia la respuestaSin recomendaciónErrorRecomendación





Nota: el diagrama representa las conexiones configuradas en el JSON. No implica que todas las ramas estén validadas funcionalmente. En particular, el ingreso por Gmail y la conexión de la memoria requieren revisión.

3. Inventario de nodos
3.1. Entrada y captura
Nodo	Tipo	Función
escucha telegram	Telegram Trigger	Recibe actualizaciones de mensajes de Telegram.
Gmail Trigger	Gmail Trigger	Recibe eventos de correo mediante polling cada minuto.
Captura info	Set	Normaliza los datos de entrada para el resto del flujo.

El nodo escucha telegram está configurado para recibir actualizaciones del tipo message.

El nodo Gmail Trigger está configurado con una frecuencia de consulta de un minuto. En el JSON no se observan filtros adicionales.

Ambos triggers están conectados al mismo nodo Captura info.
3.2. Procesamiento documental
Nodo	Tipo	Función
Search files and folders	Google Drive	Busca archivos PDF en una carpeta específica.
Download file	Google Drive	Descarga los archivos encontrados.
Default Data Loader	Document Loader	Procesa el archivo binario y lo transforma en documentos.
Embeddings Google Gemini	Embeddings	Genera representaciones vectoriales del contenido.
Supabase Vector Store	Supabase Vector Store	Inserta los documentos en la tabla documents.

3.3. Inteligencia artificial
Nodo	Modelo configurado	Función
sonora	Groq: meta-llama/llama-prompt-guard-2-22m	Clasifica la intención y selecciona música.
sonora1	Google Gemini: models/gemini-3.5-flash	Redacta la respuesta musical final.
actua frente al error	MiniMax-M3	Genera una respuesta alternativa ante errores.
registra respuesta	Ollama: beam	Gestiona el registro de interacciones.

3.4. Procesamiento y enrutamiento
Nodo	Tipo	Función
Code in JavaScript	Code	Convierte la salida JSON de SONORA en propiedades individuales.
Organiza respuesta	Code	Intenta interpretar la respuesta de SONORA1.
Enruta respuesta	Switch	Clasifica la respuesta y determina la rama siguiente.
organiza datos respuesta	Set	Prepara los campos para el envío de una respuesta general.

3.5. Integraciones auxiliares
Nodo	Tipo	Función
API Rapid	HTTP Request Tool	Consulta información de artistas y álbumes.
Listado de errores	Google Sheets Tool	Inserta registros de errores.
listado de respuestas	Google Sheets Tool	Actualiza registros de interacciones.
envia la respuesta	Telegram	Envía la respuesta final al chat.


4. Recepción y normalización de datos
4.1. Telegram
El trigger recibe el mensaje y lo entrega a Captura info.

El nodo organiza los siguientes campos:
Campo	Origen	Tipo
Chat ID	message.chat.id	Número
message.chat.first_name	message.chat.first_name	Texto
message.chat.username	message.chat.username	Texto
message.date	$now.toFormat('yyyy-MM-dd HH:mm:ss')	Texto
message.text	message.text	Texto

La fecha registrada corresponde al momento de procesamiento del mensaje, no al timestamp original recibido desde Telegram.
4.2. Gmail
El trigger de Gmail está configurado para consultar nuevos eventos cada minuto.

Sin embargo, Captura info espera una estructura con propiedades como:
$json.message.chat.id
$json.message.chat.first_name
$json.message.chat.username
$json.message.text


La estructura de un evento de Gmail es diferente. Por lo tanto, la conexión actual no permite afirmar que el ingreso por correo esté integrado correctamente.

Para incorporar Gmail como canal real, sería necesario agregar una etapa de adaptación específica para convertir el correo recibido a un formato compatible con el resto del workflow.

5. Base documental y almacenamiento vectorial
5.1. Búsqueda en Google Drive
El nodo Search files and folders realiza una búsqueda con la siguiente consulta:
'1PFhIywQDc6N6Lt9a0aVE6jn3XpFHGQnV' in parents
and mimeType = 'application/pdf'
and trashed = false


La configuración establece:
- Recurso: archivos y carpetas.
- Método: búsqueda por consulta.
- Tipo de archivo: PDF.
- Exclusión de archivos eliminados.
- Retorno: todos los resultados.
5.2. Descarga y procesamiento
El nodo Download file utiliza el ID del archivo encontrado para descargarlo.

Luego, Default Data Loader recibe el contenido binario y lo convierte en documentos procesables.

El archivo de prueba incluido en el JSON corresponde a:
- Nombre: Historia+de+la+Música.pdf.
- Formato: PDF.
- Extensión documental: 26 páginas.
- Título identificado: Introducción a la historia de la música.
- Autor del capítulo: Dietrich Schwanitz.
5.3. Embeddings y Supabase
El nodo Embeddings Google Gemini genera los vectores utilizados por Supabase.

El nodo Supabase Vector Store tiene la siguiente configuración:
Parámetro	Valor
Modo	insert
Tabla	documents
Función de consulta configurada	match_documents

La conexión recibe:
- El documento procesado desde Default Data Loader.
- Los embeddings desde Embeddings Google Gemini.
La salida principal del nodo está conectada a SONORA.

6. Primer agente: SONORA
6.1. Objetivo
SONORA es el agente encargado de interpretar el mensaje del usuario y determinar si corresponde generar una recomendación musical.

Su responsabilidad termina cuando produce una clasificación y, si corresponde, una selección de artista y álbum.

No debe redactar el mensaje final de Telegram ni realizar búsquedas externas.
6.2. Modelo
El nodo de modelo conectado es:
- Proveedor: Groq.
- Modelo: meta-llama/llama-prompt-guard-2-22m.
Este es el modelo declarado en el JSON. Su idoneidad para realizar una tarea generativa de selección musical debe validarse mediante pruebas.
6.3. Detección de intención
El prompt establece dos categorías:

A. recomienda algo para escuchar

Se utiliza cuando el usuario:
- Solicita música explícitamente.
- Pide artistas o bandas similares a otros.
- Busca música según un género.
- Expresa un estado de ánimo o actividad que requiere una recomendación.
En estos casos, SONORA debe seleccionar un artista y un álbum concreto.

B. chat sin recomendacion

Se utiliza cuando el usuario:
- Saluda o se despide.
- Realiza preguntas generales sobre Aura o la plataforma.
- Hace comentarios que no requieren una recomendación musical.
6.4. Criterios de selección
El prompt establece que el agente debe:
- Priorizar la afinidad con el pedido actual.
- Considerar los gustos y antecedentes musicales disponibles.
- Evitar repetir recomendaciones recientes.
- Considerar sonido, estilo, instrumentación, influencias y atmósfera cuando se solicitan similitudes.
- No limitarse a un único género.
- Seleccionar un álbum concreto y representativo.
- No inventar artistas ni títulos de álbumes.
6.5. Formato de salida
SONORA debe devolver exclusivamente JSON válido.

Ejemplo de recomendación:
{
  "category": "recomienda algo para escuchar",
  "artist": "Nombre del artista o banda",
  "album": "Nombre del álbum",
  "context_note": "Breve explicación de la elección"
}


Ejemplo de conversación general:
{
  "category": "chat sin recomendacion",
  "artist": null,
  "album": null,
  "context_note": null
}


6.6. Memoria conversacional
El prompt indica que SONORA debe utilizar el historial disponible para:
- Reconocer artistas, bandas y géneros mencionados.
- Identificar preferencias y rechazos.
- Considerar recomendaciones anteriores.
- Evitar repeticiones.
- Mantener continuidad.
También establece que no debe inventar recuerdos ni afirmar que guardó o actualizó información.

Configuración observada: existe un nodo Simple Memory, con:
Parámetro	Valor
Tipo	memoryBufferWindow
Clave de sesión	aura-v2- + Chat ID
Ventana de contexto	1

Sin embargo, la conexión ai_memory de ese nodo aparece sin destino. No está conectado al agente SONORA.

Por lo tanto, aunque el prompt contempla memoria conversacional, la configuración exportada no demuestra que el agente esté recibiendo ese historial.

7. Procesamiento de la salida de SONORA
El nodo Code in JavaScript recibe el campo output del agente y ejecuta:
const rawOutput = $input.item.json.output;

let parsedData;

try {
  parsedData = JSON.parse(rawOutput);
} catch (error) {
  parsedData = {
    response: rawOutput,
    category: "chat sin recomendacion"
  };
}

return [{
  json: {
    response: parsedData.response,
    category: parsedData.category
  }
}];


7.1. Función
El nodo intenta convertir la salida JSON de SONORA en propiedades individuales.
7.2. Cambio respecto de la versión anterior
En esta versión se incorporó un bloque try/catch, de modo que un JSON inválido no produce necesariamente una interrupción inmediata del nodo.
7.3. Inconsistencia en la estructura de datos
Cuando SONORA devuelve un JSON válido, sus propiedades son:
- category
- artist
- album
- context_note
Pero el código devuelve:
- response
- category
Por lo tanto, los campos artist, album y context_note se descartan.

Además, si el JSON es inválido, el código asigna el texto original a response y clasifica la salida como chat sin recomendacion.

Consecuencia: la salida de este nodo no conserva los datos que SONORA1 necesita para construir una recomendación musical.

8. Segundo agente: SONORA1
8.1. Objetivo
SONORA1 recibe una recomendación musical en bruto y tiene como objetivo enriquecerla con información complementaria y redactar una respuesta conversacional para Telegram.
8.2. Modelo
Parámetro	Valor
Proveedor	Google Gemini
Modelo	models/gemini-3.5-flash

8.3. Personalidad y tono
El prompt define a Aura como un asistente:
- Cercano y conversacional.
- Apasionado por la música.
- Con conocimiento de estilos, sonidos y producción.
- Curioso y predispuesto a continuar la charla.
Debe responder siempre en español latinoamericano, utilizando un lenguaje natural, relajado y moderno.

También establece que debe utilizar el nombre del usuario cuando esté disponible.
8.4. Datos que espera recibir
El prompt utiliza:
- artist
- album
- context_note
Además, intenta acceder al nombre del usuario mediante:
$json.message.chat.first_name


8.5. Consulta de información musical
Cuando artista y álbum no son nulos, el agente tiene instrucciones de ejecutar la herramienta API Rapid con los parámetros:
Parámetro	Valor
s	Nombre del artista
a	Nombre del álbum

La intención es obtener datos complementarios, como:
- Año de lanzamiento.
- Género.
- Canciones destacadas.
- Sello discográfico.
- Datos curiosos.
Si artista y álbum son nulos, el agente no debería ejecutar la herramienta.
8.6. Redacción de la respuesta
SONORA1 debe:
- Mencionar artista y álbum.
- Explicar por qué vale la pena escucharlo.
- Incorporar información de la API y la nota de contexto.
- Usar párrafos breves.
- Utilizar emojis con moderación.
- Mencionar Spotify, Apple Music o YouTube Music como plataformas donde buscar el álbum.
- Cerrar con una pregunta para continuar la conversación.
8.7. Formato de salida
El prompt exige exclusivamente texto conversacional, sin JSON ni metadatos.

Esto genera una incompatibilidad con el nodo siguiente, que intenta interpretar la salida como JSON. Se detalla más adelante.

9. Integración HTTP: API Rapid
El nodo API Rapid está conectado a SONORA1 como herramienta de tipo ai_tool.
9.1. Configuración observada
Parámetro	Valor
Tipo	HTTP Request Tool
Versión del nodo	4.5
URL	https://rapidapi.com
Envío de query parameters	Activado

Los parámetros configurados son:
s = $('Code in JavaScript').item.json.artist
a = $('Code in JavaScript').item.json.album


9.2. Objetivo
La herramienta pretende recuperar información del artista y del álbum para enriquecer la recomendación.

10. Organización y enrutamiento de la respuesta
10.1. Nodo Organiza respuesta
Este nodo intenta convertir la salida de SONORA1 en JSON.

Su código contempla un try/catch y, ante un error de parsing, utiliza el texto original como respuesta y asigna la categoría chat sin recomendacion.

La estructura que intenta generar es:
- response
- category
10.2. Incompatibilidad con SONORA1
SONORA1 devuelve texto conversacional, no JSON.

Por lo tanto, en condiciones normales, el código no podrá interpretar esa salida como JSON y utilizará el bloque alternativo.

Esto significa que una recomendación musical puede terminar clasificada como conversación general.
10.3. Nodo Enruta respuesta
El Switch tiene tres reglas:
Salida	Condición	Destino
Sin recomendación	category = chat sin recomendacion	organiza datos respuesta
Error	response = Error en el agente	actua frente al error
Recomienda	category = recomienda algo para escuchar	registra respuesta

La lógica depende de que Organiza respuesta entregue los campos y valores esperados.

11. Gestión de respuestas generales
La rama Sin recomendación ejecuta el nodo organiza datos respuesta.

Este nodo conserva:
- response
- category
Luego conecta directamente con envia la respuesta.

El objetivo es responder saludos, despedidas y comentarios generales sin ejecutar la persistencia de una recomendación musical.

12. Gestión de errores
12.1. Agente actua frente al error
Este agente está configurado para generar un mensaje breve, empático y tranquilizador.

El prompt establece tres objetivos:
1. Informar que ocurrió un inconveniente técnico.
2. Pedir paciencia y comprensión.
3. Comunicar que el equipo técnico está trabajando para resolverlo.
Debe responder en español latinoamericano, evitar tecnicismos y no inventar tiempos de resolución.
12.2. Modelo y memoria
Componente	Configuración
Modelo	MiniMax-M3
Memoria	Simple Memory1
Clave de sesión	ID del chat de Telegram
Ventana de contexto	30

En este caso, Simple Memory1 sí aparece conectado al agente de errores.
12.3. Registro de errores
El agente tiene conectada la herramienta Listado de errores.

La herramienta está configurada para insertar una fila nueva en:
- Documento: log de errores.
- Hoja: Hoja 1.
Los campos mapeados son:
Columna	Valor
fecha	Fecha procesada en Captura info
texto	Salida del agente sonora
13. Registro de interacciones
13.1. Agente registra respuesta
Este agente utiliza:
- Modelo Ollama beam.
- Herramienta Google Sheets listado de respuestas.
Su objetivo es guardar los datos de la interacción y devolver una confirmación estructurada.

El prompt establece que debe:
1. Analizar los campos recibidos.
2. Ejecutar la herramienta de Google Sheets.
3. Confirmar el registro.
4. Devolver un JSON con estado, mensaje y datos guardados.
13.2. Configuración de Google Sheets
La herramienta está configurada para actualizar una fila de la hoja:

Log de melomano

El documento se identifica como log de errores.

Los campos mapeados son:
Columna	Valor
Chat ID	ID del chat de Telegram
Usuario	Username
Nombre	Nombre del usuario
Mensaje	Texto original
Respuesta	Respuesta obtenida desde Enruta respuesta
time Stamp	Fecha procesada

14. Envío de respuestas a Telegram
El nodo envia la respuesta utiliza:
Parámetro	Valor
Chat ID	Captura info.Chat ID
Texto	$json.response
Parse mode	HTML

Este nodo recibe las salidas de tres ramas:
- organiza datos respuesta
- actua frente al error
- registra respuesta
Es necesario que todas ellas entreguen una propiedad response válida. La configuración actual de los agentes auxiliares no garantiza que esa propiedad exista en todos los casos.

15. Manejo de errores y reintentos
La configuración de los agentes principales presenta los siguientes valores:
Agente	Reintentos	Cantidad máxima	Comportamiento ante error
SONORA	Desactivados	2 configurado	No especificado como continuación alternativa.
SONORA1	Activados	2	continueRegularOutput
actua frente al error	No se observa configuración explícita	—	—
registra respuesta	No se observa configuración explícita



# Aura – Sistema de recomendación musical

## System Prompt del agente SONORA
```
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
```
Conclusión
Aura presenta una arquitectura modular para automatizar recomendaciones musicales por Telegram. n8n coordina los agentes, las consultas externas y la persistencia auxiliar. La solución permite una primera etapa de bajo esfuerzo de infraestructura y deja abierta la evolución hacia modelos más capaces, memoria persistente y bases de datos robustas. Antes de producción, deben validarse los contratos de salida, el endpoint de TheAudioDB y los mecanismos de persistencia.
