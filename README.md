# practica-wifi-segura
Informe de Auditoría de Red Wi-Fi Insegura

1.Introducción El objetivo de esta práctica es analizar los riesgos de utilizar el protocolo HTTP en una red Wi-Fi pública, identificar qué información puede observarse durante una solicitud web y explicar cómo una VPN puede proteger el tráfico de red. Para realizar el análisis se utilizó el sitio NeverSSL y las herramientas de desarrollador del navegador. También se propone complementar la observación con Wireshark para examinar los paquetes que circulan durante una conexión HTTP.
Sitio analizado

2.Sitio: http://neverssl.com
Host observado: majesticshininglushplan.neverssl.com
Protocolo de aplicación: HTTP
Método de solicitud: GET, si se confirma en la solicitud capturada.
Estado observado: 200 OK, según la observación realizada en el navegador. NeverSSL es un sitio pensado para permitir conexiones HTTP sin cifrado, lo que facilita estudiar la información que puede quedar expuesta durante una comunicación web.

3.Evidencia observada 3.1. Herramientas de desarrollador del navegador<img width="1133" height="745" alt="Captura de pantalla (91)" src="https://github.com/user-attachments/assets/56717cee-0a6e-4e79-b547-c66de1200a80" />

3.1. Inspección de la solicitud mediante las herramientas de desarrollador
Para inspeccionar la solicitud se utilizaron las herramientas de desarrollador del navegador:
1. Se abrió el sitio indicado.
2. Se presionó F12 para abrir las herramientas de desarrollador.
3. Se seleccionó la pestaña Network (Red).
4. Se recargó la página para registrar las solicitudes realizadas.
5. Se seleccionó la solicitud correspondiente al sitio y se revisaron sus encabezados.
Durante la observación se identificaron los siguientes datos:
ElementoResultado
URL :http://majesticshininglushplan.neverssl.com/online/
Host :majesticshininglushplan.neverssl.com
Código de estado: 200 OK
Método: HTTPGET
Protocolo :HTTP, sin cifrado TLS en esa solicitud

Entre los encabezados de la solicitud que se deben revisar se encuentran Host, User-Agent, Accept y Accept-Language, siempre que aparezcan en la captura. Estos permiten identificar el servidor solicitado, el navegador que realiza la petición y los tipos de contenido e idiomas que acepta.

Evidencia pendiente de adjuntar: una captura de la pestaña Network donde se puedan observar la URL, el método, el código de estado y los encabezados de la solicitud.
3.2. Análisis complementario con Wireshark
3.2. Análisis complementario con Wireshark

Wireshark es una herramienta que permite capturar y analizar los paquetes que circulan por una red. Para esta práctica, se utilizó el tráfico propio generado al ingresar al sitio web de NeverSSL, con el objetivo de observar cómo se realiza una solicitud HTTP.

El procedimiento consiste en abrir Wireshark, seleccionar la interfaz de red que se está utilizando e iniciar la captura. Luego, se ingresa a http://neverssl.com y se recarga la página para generar tráfico. Una vez hecho esto, se detiene la captura y se aplica el filtro http para intentar identificar los paquetes relacionados con este protocolo.

Al seleccionar una solicitud HTTP, se pueden revisar sus detalles, como el método utilizado, la ruta solicitada, el host y los encabezados, siempre que estén disponibles en los paquetes capturados. También es posible utilizar la opción Follow → TCP Stream para examinar el contenido de una conversación TCP compatible.

Es importante aclarar que los datos obtenidos deben coincidir con lo que se observa realmente en Wireshark. Por este motivo, los paquetes, encabezados y contenidos identificados deben completarse a partir de la captura realizada, sin dar por hecho que se pudieron visualizar todos esos elementos.

Este análisis complementa la inspección realizada desde las herramientas de desarrollador del navegador y permite comprender mejor cómo se transmite la información mediante HTTP. Al tratarse de un protocolo sin cifrado TLS, los datos de la solicitud y de la respuesta pueden ser legibles si se capturan correctamente y no existe otra capa de protección que los cifre.

7.Análisis de riesgos 4.1. ¿Qué protocolo utiliza el sitio? La solicitud analizada utiliza HTTP. A diferencia de HTTPS, HTTP no incorpora cifrado TLS para proteger el contenido de la comunicación. Por ese motivo, los datos de la solicitud HTTP pueden transmitirse en texto legible para alguien que consiga observar el tráfico. La disponibilidad de esa información depende de la solicitud y de los paquetes que se puedan capturar. 4.2. ¿Qué información puede observarse durante la solicitud? Entre los elementos que pueden ser visibles en una solicitud HTTP se encuentran:
URL o ruta solicitada: permite conocer qué recurso se intenta obtener.
Host: identifica el nombre del servidor al que se dirige la solicitud.
Método HTTP: por ejemplo, GET, que se utiliza habitualmente para solicitar un recurso.
Headers: aportan información adicional sobre la solicitud.
User-Agent: puede identificar el navegador y ciertos detalles del entorno del cliente, según el valor enviado. Estos datos permiten analizar cómo se establece una comunicación web. En una conexión HTTP sin cifrado, un observador con acceso al tráfico puede llegar a leer la solicitud y la respuesta. 4.3. ¿Qué riesgos existen al navegar mediante HTTP desde una red Wi-Fi pública? Una red Wi-Fi pública o no confiable puede permitir que otros participantes o un atacante con una posición adecuada en la red intenten observar el tráfico. Cuando la comunicación utiliza HTTP sin cifrado, los principales riesgos son:
Exposición de información: la URL, los encabezados y el contenido transmitido pueden quedar visibles.
Intercepción: un tercero que consiga capturar el tráfico podría analizar la información intercambiada.
Modificación de contenido: un atacante con capacidad para intervenir la comunicación podría alterar datos HTTP antes de que lleguen al destinatario.
Pérdida de privacidad: la navegación puede revelar qué recursos se solicitan y otros metadatos de la comunicación. El robo de credenciales es un riesgo posible si se transmiten por HTTP, pero no debe asumirse que ocurrió en esta práctica. El ejercicio utiliza un sitio de prueba y no requiere introducir contraseñas ni información personal. 4.4. ¿Cómo cambiaría el escenario utilizando una VPN? Una VPN (Virtual Private Network o red privada virtual) establece un túnel cifrado entre el dispositivo y el servidor VPN. Sus funciones principales son:
Cifrado: protege el tráfico dentro del túnel para que otros participantes de la red local no puedan leerlo fácilmente.
Túnel seguro: encapsula el tráfico del dispositivo y lo transporta de forma protegida hasta el servidor VPN.
Protección del tráfico: dificulta que alguien que observa la red Wi-Fi pueda interpretar o modificar directamente los datos que circulan dentro del túnel.
Privacidad: reduce la exposición del tráfico frente a otros participantes de la red local. Sin embargo, el proveedor de VPN puede observar determinados metadatos y la VPN no garantiza anonimato absoluto. Limitación importante: una VPN no convierte HTTP en HTTPS. El tramo entre el servidor VPN y el sitio web puede seguir utilizando HTTP sin cifrado. Por lo tanto, el operador de la VPN o un atacante situado en ese tramo podría llegar a observar o modificar ese tráfico. Para proteger la comunicación con el sitio web, HTTPS sigue siendo fundamental.

5. Mis tres reglas de oro para navegar en redes Wi-Fi públicas

5.1. Utilizar una VPN confiable cuando sea necesario

Usar una VPN puede ayudar a proteger mi información cuando me conecto a una red Wi-Fi pública, ya que cifra el tráfico entre mi dispositivo y el servidor VPN. Esto es útil sobre todo cuando estoy conectado a una red que no conozco o que no controlo.

5.2. Evitar conectarme automáticamente a redes públicas

Es recomendable desactivar la conexión automática a redes Wi-Fi y eliminar las redes que ya no utilizo. De esta manera, evito conectarme sin darme cuenta a una red desconocida que podría no ser segura.

5.3. Priorizar HTTPS y prestar atención a las advertencias del navegador

Siempre que navegue por internet, debo comprobar que los sitios utilicen HTTPS, ya que este protocolo protege la comunicación entre el navegador y la página web mediante el cifrado TLS. Además, si el navegador muestra una advertencia de seguridad o un problema con el certificado, no debo ignorarla ni ingresar información personal o contraseñas hasta comprobar que el sitio sea seguro.

6. Conclusión

A partir del análisis de NeverSSL, pude comprender mejor las diferencias entre HTTP y HTTPS y por qué es importante proteger la información que envío por internet. Cuando se utiliza HTTP sin cifrado, los datos transmitidos pueden quedar expuestos si alguien consigue capturar el tráfico de la red.

También pude entender que las herramientas de desarrollador del navegador permiten revisar información sobre las solicitudes web, mientras que Wireshark sirve para analizar los paquetes que circulan por la red y, cuando es posible, observar el contenido de las comunicaciones HTTP.

Por otro lado, aprendí que utilizar una VPN puede aportar una protección adicional al conectarme a una red Wi-Fi pública, aunque no reemplaza el cifrado HTTPS entre el navegador y el sitio web. En conclusión, considero que utilizar conexiones seguras, tener cuidado con las redes públicas y recurrir a una VPN confiable cuando sea necesario son medidas importantes para reducir los riesgos y navegar de forma más segura.


