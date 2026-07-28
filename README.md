# Gestión e ingesta de reclamos ciudadanos

El proceso actual de gestión de atención ciudadana opera bajo un esquema completamente manual y secuencial, donde cada correo recibido en la casilla oficial depende de la intervención directa de un operador para su apertura, lectura, interpretación y carga artesanal en Trello. Esta dinámica —que abarca desde la evaluación subjetiva del grado de urgencia hasta la redacción individualizada de respuestas— genera prolongados tiempos de espera en bandeja (de 1 a 24 horas), demanda un esfuerzo operativo considerable de entre 18 y 35 minutos por caso y conlleva una tasa de error en la clasificación del 15% al 20% debido a la fatiga y a la falta de criterios estandarizados.

[Proceso analógio (Manual)](#proceso-analógio-manual)

[Proceso propuesto automatizado (con IA)](#proceso-propuesto-automatizado-con-ia)

[Guía paso a paso para desplegar en Make.com](#guía-paso-a-paso-para-desplegar-en-makecom)

## Proceso analógio (Manual)

#### 1\. Ingesta y apertura manual del mensaje

El proceso inicia cuando un ciudadano envía un correo a la casilla oficial. Sin un sistema de detección inteligente, el mensaje permanece en la bandeja de entrada de **Gmail** hasta que un operador humano inicia su jornada y abre el correo de forma secuencial.

- **Tiempo estimado de espera en bandeja:** 1 a 24 horas (dependiendo de la hora de envío y días hábiles).

#### 2\. Lectura, interpretación y evaluación manual

Un agente de atención ciudadana lee el cuerpo completo del correo. Si la redacción del ciudadano es extensa, confusa o alterada por el enojo, el operador debe dedicar tiempo considerable a:

- Releer el texto para identificar la causa raíz del problema.
- Determinar subjetivamente qué tan "urgente" es el caso sin un criterio estandarizado unificado.
- Extraer manualmente datos como direcciones, teléfonos o nombres.

**Tasa de Procesamiento:** Entre **8 y 15 minutos** por correo para lectura, síntesis y clasificación manual.

#### 3\. Registro manual y clasificación en Trello

Una vez interpretado el correo, el operador abre la pestaña de **Trello** en su navegador y realiza la carga de forma artesanal:

1.  Crea una nueva tarjeta en la lista de entrada.
2.  Copia y pega el nombre y correo del ciudadano desde Gmail.
3.  Escribe a mano un resumen del problema.
4.  Selecciona manualmente la etiqueta de urgencia (_Rojo, Amarillo, Verde_) basándose en su criterio personal.

**Margen de Error:** Se registra una tasa de error de entrada de datos o mala clasificación de urgencia del **15% al 20%** debido a la fatiga operativa o criterios dispares entre diferentes agentes.

#### 4\. Redacción individualizada de la respuesta

El agente vuelve al caso para redactar una respuesta. Debe redactar desde cero o buscar una plantilla genérica en un documento externo, adaptándola manualmente al caso particular y cuidando de mantener un lenguaje institucional apropiado.

**Redacción:** **10 a 20 minutos** adicionales por reclamo para redactar un correo formal y personalizado.

#### 5\. Envío y gestión del estado

Finalmente, el agente envía la respuesta desde Gmail, regresa a Trello, mueve la tarjeta a la columna correspondiente ("En Proceso" o "Resuelto") y añade una nota manual con la fecha de envío.

#### Cuadro comparativo de indicadores operativos (Matriz de Eficiencia)

| **INDICADOR DE GESTIÓN** | **PROCESO ANALÓGICO (MANUAL)** | **META DEL PROCESO AUTOMATIZADO**<br>**(CON IA)** |
| --- | --- | --- |
| **Tiempo total por reclamo** | **20 a 40 minutos** por caso | **1 a 3 minutos** (solo revisión humana) |
| **Tiempo de primera respuesta (SLA)** | **24 a 72 horas hábiles** | **Menos de 2 horas** |
| **Capacidad de procesamiento** | ~15 a 20 reclamos/día por agente | **+100 reclamos/día** por agente |
| **Estandarización del criterio** | Baja (depende del humor/criterio del agente) | Alta (reglas y modelo de lenguaje unificado) |
| **Tasa de error en captura de datos** | 15% - 20% | < 2% |

## Proceso propuesto automatizado (con IA)

#### 1\. Recepción e ingesta del reclamo (**Gmail**)

El proceso comienza cuando un ciudadano envía un correo electrónico a la casilla oficial de atención ciudadana exponiendo una queja, reclamo o sugerencia. Automáticamente, un activador (_trigger_) detecta la llegada del nuevo mensaje no leído en **Gmail** y captura los metadatos esenciales: remitente, asunto, fecha y cuerpo completo del correo.

#### 2\. Procesamiento cognitivo y análisis de contenido (**Gemini**)

Sin intervención humana previa, la información extraída es enviada al modelo de IA **Gemini** para un análisis lingüístico profundo y contextual. En este paso, Gemini ejecuta cuatro tareas en paralelo:

- **Análisis de Tono y Sentimiento:** Evalúa el nivel de frustración, enojo o urgencia emocional expresado por el ciudadano para contextualizar la respuesta.
- **Extracción de Puntos Clave:** Resume el conflicto central en viñetas concretas, identificando ubicaciones, servicios afectados o fechas clave mencionadas.
- **Clasificación de Urgencia:** Basándose en criterios predefinidos de impacto operativo y riesgo, categoriza el reclamo en uno de los tres niveles:
    - 🔴 **Urgente:** Riesgo inminente a la salud, seguridad o infraestructura crítica (ej. fuga de gas, semáforo descompuesto en avenida principal).
    - 🟡 **Poco Urgente:** Problemas operativos menores sin riesgo inmediato (ej. luminaria apilada, poda de árbol en parque).
    - 🟢 **Nada Urgente:** Consultas de trámite, sugerencias generales o reclamos estéticos.
- **Generación de Borrador de Respuesta:** Redacta una propuesta de respuesta oficial personalizada, manteniendo un tono empático, institucional y claro, adaptado a la situación particular y al estado emocional del ciudadano.

#### 3\. Creación y organización de la tarjeta (**Trello**)

Una vez procesados los datos por Gemini, el flujo utiliza la API de **Trello** para dar de alta automáticamente una nueva tarjeta en la columna de **Backlog**:

- **Título de la tarjeta:** \[Categoría de Urgencia\] + Breve resumen del problema + (Nombre del Remitente).
- **Etiquetas (_Labels_):** Se asigna un código de color según la urgencia (Rojo = Urgente, Amarillo = Poco Urgente, Verde = Nada Urgente) para facilitar la priorización visual del equipo operativo.
- **Descripción de la tarjeta:** Incluye la información procesada estructurada de la siguiente manera:
    - **Datos del Remitente:** Nombre y correo electrónico.
    - **Resumen de Puntos Clave:** Puntos principales del reclamo.
    - **Análisis de Tono:** Diagnóstico emocional (ej. _"Tono: Altamente frustrado"_).
    - **Borrador de Respuesta Oficial:** Texto listo para ser revisado por un agente humano.
- **Archivo adjunto / Enlace:** Un enlace directo al correo original en Gmail por si se requiere auditar el texto completo.

#### 4\. Revisión humana y cierre (Human-in-the-loop)

El operador humano asignado al tablero de Trello recibe la tarjeta en el backlog, revisa la información sintetizada y el borrador propuesto. Con solo ajustar o aprobar el borrador, el agente puede proceder al envío final al ciudadano y mover la tarjeta a la columna correspondiente ("En Proceso" o "Resuelto"), garantizando una atención ágil, estandarizada y priorizada.

## Guía paso a paso para desplegar en Make.com

#### Paso 1: Configurar el Trigger (**Gmail**)

El objetivo de este módulo es escuchar la llegada de nuevos reclamos a la bandeja de atención ciudadana.

1.  Añade el módulo **Gmail - Watch Emails** (o _Watch Emails in a Folder/Label_).
2.  **Parámetros de configuración:**

- **Folder:** Selecciona INBOX (o la carpeta/etiqueta específica de reclamos).
- **Criteria:** Configúralo como UNSEEN (solo correos no leídos) para evitar procesar correos duplicados.
- **Maximum number of results:** Establece un límite adecuado por ciclo (ej. 1 a 5).

1.  Guarda la configuración y realiza una prueba con un correo de ejemplo.

#### Paso 2: Análisis Cognitivo del Correo (**Google Gemini**)

En este paso, la IA lee el contenido del correo, identifica el problema, analiza el tono del ciudadano, sugiere una respuesta oficial y determina la urgencia del caso.

1.  Agrega el módulo **Google Gemini - Create a Prompt / Generate Content** inmediatamente después de Gmail.
2.  **Conexión:** Asocia tu API Key gratuita de Google AI Studio (studio.google.com).
3.  **Modelo:** Selecciona gemini-2.5-flash o gemini-1.5-flash.
4.  **Prompt del Sistema (System Instruction):**  
    Copia y pega la instrucción de comportamiento del modelo:

```
Eres un asistente especializado en atención ciudadana. Analiza el correo recibido y devuelve ÚNICAMENTE una cadena en formato JSON válido sin bloques Markdown de código. 
Estructura requerida:  

{  
"tono_sentimiento": "Diagnóstico emocional (ej. Altamente frustrado)",  
"puntos_clave": "Resumen en viñetas del conflicto central",  
"urgencia": "Urgente | Poco Urgente | Nada Urgente",  
"resumen_problema": "Breve resumen de 5 palabras del problema",  
"borrador_respuesta": "Propuesta de respuesta institucional, empática y clara"  
}  

Criterios de clasificación de Urgencia:  
- Urgente: Riesgo inminente a la salud, seguridad o infraestructura crítica (ej. fuga de gas, semáforo descompuesto).  
- Poco Urgente: Problemas operativos menores sin riesgo inmediato (ej. luminaria apagada, poda de árbol).  
- Nada Urgente: Consultas de trámite, sugerencias generales o reclamos estéticos.
```

1.  **Prompt del Usuario (User Message):**  
    Mapea los datos provenientes del **Módulo 1 (Gmail)**:
    - **Texto de entrada:**  
        Asunto: {{1.subject}}  
        Cuerpo del correo: {{1.text}}
2.  **Formato de Respuesta:** Configura el parámetro responseMimeType a application/json dentro de generationConfig.

### Paso 3: Parsear la Respuesta de la IA (JSON Parser)

Dado que Gemini devuelve una cadena de texto en formato JSON, debemos estructurarla para que Make.com la interprete como variables independientes.

1.  Conecta el módulo nativo **Tools - JSON Parser**.
2.  **JSON String:** Mapea la salida de texto o resultado del **Módulo 2 (Gemini)**: {{2.result}}.
3.  Ejecuta el módulo una vez manualmente para que Make detecte la estructura del objeto devuelto (urgencia, tono_sentimiento, puntos_clave, resumen_problema, borrador_respuesta).

### Paso 4: Bifurcación del Flujo (Router y Filtros)

El Router evaluará el nivel de urgencia extraído por el Parser y dirigirá el flujo hacia la rama correspondiente.

1.  Añade un módulo **Flow Control - Router** después del JSON Parser.
2.  El Router creará **3 ramas de salida** (Rama A, Rama B, Rama C).

#### Configuración de los Filtros en las Conexiones:

- **Filtro de la Rama A (Urgente):**
    - **Nombre:** Urgencia == Urgente
    - **Condición:** Mapea el campo {{3.urgencia}} del Parser.
    - **Operador:** Text operator: Equal to (case-insensitive).
    - **Valor:** Urgente.
- **Filtro de la Rama B (Poco Urgente):**
    - **Nombre:** Urgencia == Poco Urgente
    - **Condición:** Mapea {{3.urgencia}}.
    - **Operador:** Text operator: Equal to (case-insensitive).
    - **Valor:** Poco Urgente.
- **Filtro de la Rama C (Nada Urgente):**
    - **Nombre:** Urgencia == Nada Urgente
    - **Condición:** Mapea {{3.urgencia}}.
    - **Operador:** Text operator: Equal to (case-insensitive).
    - **Valor:** Nada Urgente.

### Paso 5: Creación de Tarjetas Priorizadas en Trello

Cada rama del Router terminará en un módulo de Trello configurado para crear una tarjeta en la columna **Backlog**, pero diferenciándose en el color de la etiqueta.

1.  Agrega el módulo **Trello - Create a Card** al final de cada una de las 3 ramas.
2.  **Configuración compartida para los módulos 5A, 5B y 5C:**

- **Board ID:** Selecciona tu Tablero de Atención Ciudadana.
- **List ID:** Selecciona la lista o columna **Backlog**.
- **Name (Título de la tarjeta):**  
    \[{{3.urgencia}}\] - {{3.resumen_problema}} - ({{1.sender.name}})
- **Description (Cuerpo de la tarjeta):**  
    Construye la plantilla en formato Markdown mezclando datos del correo (Módulo 1) y del análisis de IA (Módulo 3):  
    **Datos del Remitente:** {{1.sender.name}} ({{1.sender.email}})  
    **Puntos Clave:**  
    {{3.puntos_\_clave}}  
    _**_Análisis de Tono:_** _{{3.tono__sentimiento}}  
    **Borrador de Respuesta Oficial:**  
    {{3.borrador_\_respuesta}}  
    _**_Enlace al Correo Original / ID de Auditoría:_** _Gmail ID {{1.id}}_

1.  **Configuración específica de Etiquetas (Labels):**

- **Módulo 5A (Rama Urgente):** Asigna la etiqueta de color **Rojo** 🔴.
- **Módulo 5B (Rama Poco Urgente):** Asigna la etiqueta de color **Amarillo** 🟡.
- **Módulo 5C (Rama Nada Urgente):** Asigna la etiqueta de color **Verde** 🟢.

## Pruebas y Validación

1.  **Enviar correos de prueba:**

- Envia un correo simulando un riesgo inminente (ej. _"Hay una fuga de gas en la avenida principal"_).
- Envía un correo operativo menor (ej. _"La luz del parque no enciende desde ayer"_).
- Envía una sugerencia general (ej. _"Deberían agregar más bancas en la plaza central"_).

1.  **Ejecutar el escenario:** Haz clic en **Run once** en el editor de Make.
2.  **Verificación:** Inspecciona las tarjetas generadas en Trello. Confirma que cada una tenga la etiqueta de color adecuada, el borrador de respuesta institucional redactado y el enlace al ID del correo original.
3.  **Activación:** Activa la automatización con la llave de encendido (**ON**) para que responda automáticamente en tiempo real.

## Guía Paso a Paso: Integrar la API Key de Google Gemini en Make.com

### Paso 1: Obtener la API Key en Google AI Studio

1.  **Acceder a Google AI Studio:**
    - Ingresa a [](https://www.google.com/search?q=https://studio.google.com)[aistudio.google.com](https://aistudio.google.com) en tu navegador.
    - Inicia sesión con tu cuenta de correo de Google (Gmail).
2.  **Crear o seleccionar un Proyecto:**
    - Haz clic en el botón **"Get API key"**.
    - Si no tienes un proyecto previo, selecciona la opción **"Create API key in new project"** (o selecciona un proyecto existente de Google Cloud).
3.  **Generar y copiar la clave:**
    - Una vez creada, la plataforma te mostrará una cadena alfanumérica larga.
    - Haz clic en **Copy** para guardarla en tu portapapeles.

**Nota de seguridad:** Trata tu API Key como si fuera una contraseña personal. Nunca la compartas públicamente ni la incluyas directamente en archivos de código abiertos.

### Paso 2: Asociar la API Key dentro de Make.com

Puedes vincular tu clave tanto en los módulos de IA estándar de Gemini como en el módulo **Make AI Agent (New)**.

#### Opción A: En un módulo estándar de Gemini (ej. Create a Prompt / Generate Content)

1.  Abre tu escenario en Make.com.
2.  Agrega el módulo Google Gemini AI ➜Create a Prompt / Generate Content (o _Create a completion_).
3.  En la ventana emergente de configuración del módulo, busca el campo **Connection** y haz clic en **Add**.
4.  En la casilla **API Key**, pega la clave que copiaste desde Google AI Studio.
5.  Asigna un nombre claro a tu conexión (ej. Mi Conexión Gemini Gratis) y haz clic en **Save**.

#### Opción B: En el módulo Make AI Agent (New)

1.  En tu escenario de Make, añade el módulo Make AI Agent (New) ➜ Run an agent.
2.  En la sección **Connection**, haz clic en **Add**.
3.  En **Connection type**, selecciona **Google Gemini AI** (o _Custom AI provider_).
4.  Pega tu **API Key** en el campo correspondiente y asigna un nombre a la conexión.
5.  Haz clic en **Save**.
6.  En el desplegable **Model**, selecciona un modelo compatible como gemini-2.5-flash o gemini-1.5-flash.

### Paso 3: Validar la Conexión

1.  Guarda los cambios en tu módulo\[cite: 8\].
2.  Haz clic con el botón derecho sobre el módulo de Gemini o del Agente y selecciona **Run this module only** (Ejecutar solo este módulo).
3.  Si el módulo muestra una palomita verde ✔️ y devuelve un resultado sin errores, tu API Key ha quedado correctamente vinculada y lista para usarse en cualquier automatización.

## Guía Paso a Paso: Integrar Trello en Make.com

### Paso 1: Iniciar la vinculación desde Make.com

1.  Abre tu escenario en Make.com y entra a la ventana de configuración del módulo de **Trello** (por ejemplo, Create a Card).
2.  En la parte superior de la ventana del módulo, ubica el campo **Connection**.
3.  Haz clic en el botón azul **Add** (Agregar) situado a la derecha del menú desplegable.

### Paso 2: Nombrar y autorizar la conexión

1.  En la ventana emergente que se abre, asigna un nombre reconocible para tu conexión en el campo **Connection name** (por ejemplo, Trello - Atención Ciudadana).
2.  Haz clic en el botón **Save** (o _Continue_).
3.  Make.com te redirigirá automáticamente a una ventana segura de autenticación de **Trello** (Atlassian). Si aún no has iniciado sesión en Trello en tu navegador, ingresa tus credenciales (correo y contraseña).

### Paso 3: Otorgar permisos de acceso

1.  Trello mostrará una pantalla de autorización informando los permisos que Make.com solicita (lectura y escritura de tableros, listas y tarjetas).
2.  Desplázate hacia el final de la página y haz clic en el botón verde **Allow** (Permitir).
3.  Una vez autorizado el acceso, la ventana se cerrará automáticamente y volverás al panel de configuración del módulo en Make.com.

### Paso 4: Seleccionar Tablero, Lista y Etiquetas

Con la conexión activa, Make cargará dinámicamente los datos de tu cuenta de Trello:

1.  **Board (Tablero):** Haz clic en el desplegable y selecciona el tablero donde se gestionarán los reclamos (ej. _Atención Ciudadana_).
2.  **List (Lista):** Selecciona la columna o lista de destino (ej. _Lista de tareas_).
3.  **Labels (Etiquetas):** Elige la etiqueta correspondiente según el nivel de urgencia de la rama del flujo (por ejemplo, la etiqueta **Roja** para la rama Urgente, **Amarilla** para Poco Urgente o **Verde** para Nada Urgente).
