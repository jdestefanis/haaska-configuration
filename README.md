# haaska-configuration
Configure Haaska - Configurar Haaska para Home Assistant - Home Assistant Alexa Skill Adapter

[![Build Status](https://travis-ci.org/mike-grant/haaska.svg?branch=master)](https://travis-ci.org/mike-grant/haaska)

---
### English

haaska implements a bridge between the [Home Assistant Smart Home API](https://www.home-assistant.io/components/alexa/#smart-home) and the [Alexa Smart Home Skill API](https://developer.amazon.com/alexa/smart-home) from Amazon.

This provides voice control for a connected home managed by Home Assistant, through any Alexa-enabled device.

### Getting Started
To get started, head over to the [haaska Wiki](https://github.com/jdestefanis/wiki).




### Español
haaska implementa un puente entre [Home Assistant Smart Home API](https://www.home-assistant.io/components/alexa/#smart-home) y Alexa [Alexa Smart Home Skill API](https://developer.amazon.com/alexa/smart-home) de Amazon.

Esta integracion provee control de voz para todos los dispositivos que estan controlados y administrados por Home Assistant y cualquier dispositivo Alexa

### Empezando

Antes de instalar y configurar haaska, hay algunas cosas que debe configurar y tener listas. Sin estos, el servicio Alexa Voice de Amazon y haaska no funcionarán.

Versión
Debe estar ejecutando Home Assistant 0.78 o posterior. Las versiones anteriores ya no son compatibles con esta última versión.

Reenvío de puertos
Debe tener configurado el reenvío de puertos para su instalación de Home Assistant. Si bien no podemos proporcionar instrucciones para cada instalación, hay varias guías disponibles en línea. Puedes probar WikiHow o PortForward.com. Por lo general, reenviará el puerto 8123 o 443. Esto se basa en el puerto que use para acceder a Home Assistant, como https://myhass:8123 o similar.

HTTPS/SSL y DNS dinámico
Debe utilizar HTTPS/SSL. Este es un requisito de Amazon y no se puede omitir. Hay varias guías para hacer esto con servicios gratuitos como Let's Encrypt. Si su conexión/instalación de Home Assistant no tiene una IP estática a Internet (la mayoría de las instalaciones no la tendrán), entonces deberá usar un servicio de DNS dinámico.

Existen varios servicios de DNS dinámico, así como diferentes formas de agregar un certificado SSL para configurar HTTPS.

Para configurar ambos, recomendamos la publicación del blog de Home Assistant: Cifrado sin esfuerzo con Let's Encrypt y DuckDNS.

Nota Hass.io
The Wanderer también proporciona una guía de instalación que menciona la configuración de DuckDNS y Let's Encrypt para Hass.io. Puedes leer sobre esto aquí: Hass.io y Alexa. Recomendamos seguir los pasos 2.1 a 2.7. El resto de los pasos los cubren nuestros guías.

Certificados autofirmados
Estos no son fácilmente compatibles. Si es posible, utilice Let's Encrypt. Si insiste en un certificado autofirmado, durante la configuración, deberá configurar SSL Verify en "falso". Hay formas de agregar su certificado, pero esto no se trata en esta guía.

Desarrollador de Amazon y cuenta de AWS
Deberá tener un desarrollador de Amazon válido y una cuenta de AWS. Estos son gratuitos para registrarse.

Para Amazon Developer, vaya a su página (developer.amazon.com) y haga clic en "Iniciar sesión". Desde allí, puede hacer clic en "Crear su cuenta de desarrollador de Amazon".

Para Amazon Web Services (AWS), vaya a su página (aws.amazon.com) y haga clic en "Registrarse". Le pedirá una tarjeta de crédito, pero no se preocupe, AWS incluye un "nivel gratuito". El servicio que utiliza haaska, AWS Lambda, permitirá hasta 1.000.000 de solicitudes, al mes, gratis. Puede leer más aquí: AWS Lambda - Precios.

Por su propia seguridad, le animamos a consultar Facturación y establecer límites/presupuestos, por si acaso. Si utiliza otros servicios de AWS, es posible que estén sujetos a un costo.


### Descargando y preparando Haaska
Hay dos formas de descargar y preparar haaska. Existe el Método Simple (recomendado), y existe el Método Avanzado.

Método simple - Recomendado
Sigue estos pasos:

Obtenga la ultima release de haaska. Esto generalmente se denomina como haaska_1.1.0.zip, no los archivos de código fuente. https://github.com/mike-grant/haaska/releases/latest

Agregue lo siguiente a su archivo configuration.yaml:

```
api:

alexa:
  smart_home:
```  
  
Obtenga un token de larga duración. Puedes crear uno dentro de Home Assistant con estos pasos:

Inicie sesión en su instalación de Home Assistant
Haga clic en la letra dentro del círculo azul en la parte superior izquierda. Esto te llevará a "Perfil".
Desplácese hacia abajo hasta "Tokens de acceso de larga duración". Haga clic en CREAR TOKEN.
¿Por nombre? ingrese haaska y haga clic en Aceptar.

<img width="800" alt="Perfil_–_Home_Assistant_and_Minimal_Dynamic_DNS_configuration_for_No-IP_com_with_ddclient___by_Nobuto_Murata___Medium" src="https://user-images.githubusercontent.com/8029197/158253797-74b60a3a-bc27-46c7-b9b9-21aef3772cb7.png">


Aparecerá una ficha. Cópielo en un lugar seguro, como un archivo de texto en el Bloc de notas. Lo necesitaremos más tarde.
¡Eso es todo! Pase a la siguiente sección, Configuración.


### Configuracion

Aquí está la parte larga. Por favor, siga estos pasos con mucho cuidado.

Recuerde revisar los pasos anteriores, Antes de comenzar y Descarga y preparación para haaska. Sin los completados, no podrá continuar.

Todos estos pasos fueron correctos a partir del 18 de febrero de 2019. Si algo ha cambiado, infórmenos a través de nuestro "¿Necesita ayuda?" sección.

Configurar inicio de sesión con Amazon

* Inicie sesión en la Consola de desarrollador de Amazon. https://developer.amazon.com/settings/console/securityprofile/overview.html
* Haga clic en el enlace "Iniciar sesión con Amazon" en la barra de navegación superior.
* Haga clic en el botón dorado que dice "Crear un nuevo perfil de seguridad".
* Ingrese cualquier nombre de perfil de seguridad que desee, p. "haaska"
* Escriba una breve descripción, p. "haaska para mi Home Assistant"

<img width="1450" alt="https___developer_amazon_com_settings_console_securityprofile_create-security-profile_html" src="https://user-images.githubusercontent.com/8029197/158254562-c62cb10b-43e2-4998-a9d3-782ff9eff446.png">

* Agregue cualquier URL que desee para el aviso de privacidad, p. "https://myhass.ejemplo.com"
* Haga clic en "Guardar" para continuar.
* En su nuevo perfil de seguridad, haga clic en el botón de engranaje y elija "Configuración web".
* Tome nota de la identificación del cliente y el secreto del cliente. Mantenga esta ventana abierta como referencia.

<img width="1453" alt="https___developer_amazon_com_settings_console_securityprofile_web-settings_view_html_showDeleted_false_identityAppFamilyId_amzn1_application_ba51793d5f304d449db70ae7a0984b33" src="https://user-images.githubusercontent.com/8029197/158254849-1d7803d7-b8d4-4a9a-82a2-1c2e602d945d.png">


### Configurar el kit de habilidades de Alexa

* Abra una nueva ventana del navegador, manteniendo la anterior abierta para más adelante.
* Haga clic en el enlace "Consola de desarrollador": https://developer.amazon.com/home.html en la parte superior derecha de la página.
* Busque "Alexa" en la barra de navegación y elija "Alexa Skills Kit". https://developer.amazon.com/alexa/console/ask
* Haga clic en el botón "Crear habilidad".

<img width="1279" alt="Amazon_Alexa_Console_-_Amazon_Alexa_Official_Site" src="https://user-images.githubusercontent.com/8029197/158255430-07a092e9-a83d-41b2-8261-5e8f217d98a1.png">

 

### En esta nueva página, ingrese lo siguiente:

* Nombre de la habilidad: puede ser cualquier cosa que desee, p. "haaska"
* Idioma: elija el idioma correcto que le gustaría usar, p. "Spanish (US)" 
* Haga clic en el mosaico "Hogar inteligente"

<img width="1192" alt="Developer_Console" src="https://user-images.githubusercontent.com/8029197/158255620-f3b0c7c4-2f05-4f0d-b91d-60967d9fe3df.png">

* Haga clic en el botón azul "Crear habilidad" en la parte superior derecha.
* Tome nota de la ID de habilidad que aparece (por ejemplo, amzn1.ask.skill.ed66dfa4-1185-492e-bf6e-1f70e90fb018).

<img width="1539" alt="Developer_Console" src="https://user-images.githubusercontent.com/8029197/158255804-9833a657-3cf4-42bd-a953-a9efdd7f5796.png">


### Preparación del acceso a AWS Lambda

* Abra una nueva ventana del navegador. Mantenga las ventanas anteriores abiertas para más adelante.
* Inicie sesión en la consola de AWS. https://console.aws.amazon.com
* Haga clic en el botón "Servicios" en la parte superior izquierda y, en la lista, busque la sección "Seguridad, identidad y cumplimiento".
* Haga clic en "IAM" en esta sección. https://console.aws.amazon.com/iam/home
* En la barra lateral izquierda, haga clic en "Roles". Luego, haga clic en el botón azul "Crear rol". https://console.aws.amazon.com/iam/home#/roles

<img width="1712" alt="IAM_Management_Console" src="https://user-images.githubusercontent.com/8029197/158256150-5b577a8a-206b-461c-b062-fc9591994cae.png">

* En esta página, asegúrese de que el mosaico "Servicio de AWS" esté seleccionado. Haga clic en "Lambda", luego haga clic en el botón "Siguiente: Permisos" en la parte inferior derecha.

<img width="1667" alt="IAM_Management_Console" src="https://user-images.githubusercontent.com/8029197/158256303-8a1b700d-a87f-4d80-8a89-56f5cb797b23.png">


* En esta nueva página, ingrese "basic" en el cuadro de búsqueda. Cuando aparezca, marque la casilla junto a "AWSLambdaBasicExecutionRole".
* Haga clic en el botón "Siguiente: Etiquetas" en la parte inferior derecha. No introduzca ninguna etiqueta. Haga clic en "Siguiente: Revisar" para continuar.
* En el cuadro de "Nombre de la función", ingrese "lambda_basic_execution". Haga clic en "Crear rol" en la parte inferior derecha.

* Haga clic en el botón AWS en la parte superior izquierda para volver a la consola principal. https://console.aws.amazon.com/console/home


<img width="1418" alt="IAM_Management_Console" src="https://user-images.githubusercontent.com/8029197/158256544-e7a30743-cd02-4433-8a3c-e5255d8283cc.png">

### Region Select
Este paso es muy importante.

En la parte superior derecha de la pantalla, hay un botón junto a su nombre de usuario, pero antes del botón "Soporte". Puede tener el nombre de una región. Debe hacer clic en él y seleccionar la región de función Lambda correcta de acuerdo con su idioma de Alexa.

Amazon proporciona una lista de regiones e idiomas. Aquí hay una copia:

<img width="818" alt="Setting_up_haaska_·_mike-grant_haaska_Wiki" src="https://user-images.githubusercontent.com/8029197/158256807-5d643107-e502-45ff-a093-77785f60525e.png">

Por ejemplo, me encuentro en Argentina y mis dispositivos Amazon Echo están configurados en "Spanish (US)". Para este paso, elegiría "US East (N. Virginia)".

### Configurando funcion AWS Lambda - Parte 1

* Haga clic en el botón Servicios en la parte superior izquierda. En la lista, busque la sección "Calcular" y haga clic en "Lambda". https://console.aws.amazon.com/lambda/home?#/create

* Haga clic en el botón naranja "Crear función".
<img width="1715" alt="Functions_-_Lambda" src="https://user-images.githubusercontent.com/8029197/158257170-7b7eb2a3-097a-4b4f-afa8-d2841936345d.png">

* Asegúrate de que el mosaico "Author from scratch" esté seleccionado.
* Configure las siguientes opciones:

Nombre - "haaska"
Tiempo de ejecución - Python 3.6
Rol - "Elegir un rol existente"
Función existente: "lambda_basic_execution"
Haga clic en el botón "Crear función".

<img width="1702" alt="Lambda" src="https://user-images.githubusercontent.com/8029197/158257399-54432716-06cf-4efd-8366-9e926737f90d.png">

### Configurando funcion AWS Lambda - Parte 2

* En la sección "Diseñador" de su nueva función, haga clic en "Alexa Smart Home". Si no ve esto, entonces no ha configurado la región correcta. https://github.com/mike-grant/haaska/wiki/Setting-up-haaska#Region-Select
* Aparecerá una sección titulada "Configurar disparadores". En el cuadro "ID de la aplicación", copie y pegue el "ID de la habilidad" de la ventana de la consola de desarrollador de Alexa (por ejemplo, amzn1.ask.skill.ed66dfa4-1185-492e-bf6e-1f70e90fb018).
* Asegúrese de que "Habilitar disparador(Triggers)" esté marcado. Haga clic en "Agregar" en la parte inferior derecha.
* En la sección "Diseñador", haga clic en el nombre de su función (por ejemplo, "haaska").
* En la sección "Código de función", busque "Tipo de entrada de código". Haga clic en este menú y seleccione "Cargar un archivo .zip".
* Haga clic en el botón "Cargar" y seleccione el archivo zip que descargamos anteriormente (por ejemplo, haaska_1.1.0.zip).
* En el cuadro "Manejador", reemplace lo que ya está allí con "haaska.event_handler".
* Haga clic en "Guardar" en la parte superior derecha de la página y espere a que se cargue.

### Configurando la funcion de Haaska

* En la sección "Código de función", verá una lista de archivos. Haga doble clic en "config.json".
* En la sección URL, agregue la URL de su Home Assistant remoto (por ejemplo, https://my-hass.example.com:8123)
* En la sección Bearer Token, agregue su Long Lived Token que guardó anteriormente (por ejemplo, "amcb3i2248yfm...")
* Haga clic en "Guardar" en la parte superior derecha.

### Linkeando AWS Lambda con Alexa Skill Kit

* En la parte superior derecha de la ventana de Lambda, hay un ARN (por ejemplo, "arn:aws:lambda:us-east-1:111234567890:function:haaska"). Copie este texto y guárdelo.
* Vuelve a la ventana de la consola de desarrollador de Alexa. Pegue el ARN en el cuadro denominado "Punto final predeterminado".
MUY IMPORTANTE. En las casillas de verificación disponibles, haga clic en la "Endpoint Regionl" que coincida con su idioma de habilidad/región de función Lambda anterior.
* Pegue el ARN en el cuadro de la región.
* Haga clic en el botón "Guardar" en la parte superior derecha.


### Linkeando Alexa con Amazon

* Haga clic en el botón "Configurar vinculación de cuentas" en la parte inferior de la página.
Introduzca la siguiente:

<img width="765" alt="Setting_up_haaska_·_mike-grant_haaska_Wiki" src="https://user-images.githubusercontent.com/8029197/158258110-72b1260f-245c-448a-956e-5ba39268e73a.png">

* Verá una lista de URI de redirección. Por favor, cópielos.
* Haz clic en Guardar en la parte superior derecha.
* Vuelva a la ventana Amazon Developer Console/Iniciar sesión con Amazon. Haz clic en Editar en la parte inferior derecha.
* En la sección "URL de retorno permitidas", haga clic en "Agregar otra" hasta que tenga tres líneas.
* En cada línea, agregue uno de los URI de redirección de la ventana "Consola de desarrollador de Alexa". Clic en Guardar.


Eso es todo!!!! Para testear la integracion mira a continuacion

### Testeando Haaska

Uso de la consola Lambda

Utilice la interfaz de Lambda para probar que haaska puede llegar a su servicio.

* Inicie sesión en la consola de Lambda. https://console.aws.amazon.com/lambda/home
* Haga clic en su función "haaska".
* Haga clic en el botón "Probar" en la parte superior derecha de la página.
* Dale a la prueba un nombre personalizado en el campo "Nombre del evento". Copie el siguiente JSON en la consola de prueba:

```
{
  "directive": {
    "header": {
      "namespace": "Alexa.Discovery",
      "name": "Discover",
      "payloadVersion": "3",
      "messageId": "1bd5d003-31b9-476f-ad03-71d471922820"
    },
    "payload": {
      "scope": {
        "type": "BearerToken",
        "token": "access-token-from-skill"
      }
    }
  }
}
```

* Haga clic en "Crear" y su prueba ahora debería aparecer en el cuadro desplegable junto al botón "Prueba".
* Haga clic en el botón "Probar" y, si la prueba tiene éxito, aparecerá una marca de verificación verde. ¡Felicidades! Su función haaska puede comunicarse con su instalación de Home Assistant.

Si hay un problema y la prueba falla, expanda la sección "Detalles". Copie el contenido del cuadro de texto de resultado y vea nuestro "¿Necesita ayuda?" sección.

### Thanks and Acknowledgement

Thanks to [@auchter](https://github.com/auchter) for creating the original haaska.

Thanks to [@bitglue](https://github.com/bitglue) for his work in getting the Smart Home API exposed via HTTP, making this slimmed down version possible.

This fork of haaska was created by [@mike-grant](https://github.com/mike-grant).

Documentation and additional maintenance is done by [@anthonylavado](https://github.com/anthonylavado), and contributors like you.

### License
haaska is provided under the [MIT License](LICENSE).
