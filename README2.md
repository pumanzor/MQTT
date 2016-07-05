2.4.1.	MQTT Protocol

MQTT (Message Queuing Telemetry Transport) es un protocolo de conectividad para sistemas M2M, su funcionamiento se basa en mensajes publish/subscribe extremadamente simple y liviano, fue diseñado para dispositivos que funcionan con restricciones de ancho de banda, alta latencia o redes poco confiables, dado lo anterior dentro de sus características implementa un cierto grado de garantía en la entrega de los mensajes.

2.4.1.1.	Arquitectura

MQTT tiene un modelo cliente/servidor, en el que cada sensor es un cliente y se conecta a un servidor conocido como “bróker” sobre TCP. 
MQTT está orientado a mensajes y cada uno de ellos es un trozo o “chunk” discreto de datos, cada mensaje se publica en una dirección conocida como tema o “topic”, los clientes pueden subscribirse a uno o a múltiples temas, cada cliente suscrito a uno de estos temas o tópicos recibe todos los mensajes publicados en ese mismo tema.

Por ejemplo, imaginemos una red simple con 3 clientes y un bróker central, los 3 clientes abren conexiones TCP con el bróker, los clientes B y C se suscriben al tópico “temperature”

 

En determinado momento, el cliente A publica un valor de 22,5 en el tópico “temperature” , el bróker reenvía el mensaje a todos los clientes suscritos.

 

El modelo Publisher/subscriber permite a los clientes MQTT comunicación uno-a-uno , uno-a-muchos, muchos-a-uno.

2.4.1.2.	Topic Matching

En MQTT, los temas son jerárquicos como en un sistema de archivos (por ejemplo, cocina/horno/temperatura), se permiten comodines cuando se registra un suscripción (pero no cuando se publica) que permite jerarquías enteras para ser observadas por los clientes.
El wilcard + coincide con cualquier nombre de directorio único, # coincide con cualquier numero de directorios o con cualquier nombre.

Por ejemplo el tópico cocina/+/temperatura coincide con cocina/foo/temperatura pero no con cocina/foo/bar/temperatura
cocina/#  coincide con cocina/refrigerador/compresor/valvula/temperatura

2.4.1.3.	Ultima voluntad y testamento

Los clientes MQTT pueden registrar un mensaje personalizado denominado “Last Will and Testament” para ser enviado al bróker si se desconectan. Estos mensajes pueden ser usados para señalar o avisar a los suscriptores cuando un dispositivo se desconecta.

2.4.1.4.	Persistencia

MQTT tiene soporte para mensajes persistentes almacenados en el bróker. Al publicar mensajes los clientes pueden solicitar que el bróker persista el mensaje. Solo el más reciente mensaje persistente es almacenado, cuando un cliente se suscribe a un tópico, cualquier mensaje persistido se enviará al cliente.

2.4.1.5.	Seguridad

Los brokers MQTT pueden requerir autenticación mediante usuario y clave a los clientes que se conecten ya sea para suscribir o publicar, para asegurar la privacidad la conexión TCP puede ser encriptada mediante SSL/TLS.

2.4.1.6.	MQTT-SN

A pesar de que MQTT está diseñado para ser ligero, tiene 2 inconvenientes en especial para dispositivos muy restringidos.
Cada cliente MQTT debe soportar y ser compatible con TCP y normalmente mantener una conexión abierta con su bróker en todo momento. Para algunos entornos en donde la perdida de paquetes es alta o los recursos de computación son escasos puede traer complicaciones.
Los nombres de temas o tópicos en MQTT son a menudo cadenas bastante largas lo cual lo hace poco práctico para normas como 802.15.4
Ambas deficiencias son solucionadas mediante el uso del protocolo MQTT-SN, que define un mapeo en UDP de MQTT y añade soporte al bróker para indexar nombres de temas o tópicos

2.4.1.7.	Principales características

•	Transporte de mensajes que es indepediente al contenido de la carga util o payload.
•	Tres tipos de QoS para entrega de mensajes
o	At most once' o QoS = 0, donde los mensajes son entregados segun el mejor esfuerzo por lo tanto pueden darse situaciones de perdidas de mensajes. Este nivel puede ser usado por ejemplo; en situaciones en que una lectura individual de datos de un sensor de ambiente se pierde sabiendo que en la proxima se publicara poco despues.
o	'At least onde' o QoS =1, donde se asegura que los mensajes llegaran a destino pero duplicados pueden ocurrir.
o	'Exactly once' o QoS =3, donde se asegura que los mensajes llegaran a destino exactamente 1 vez. Este nivel podría ser utilizado por ejemplo, con sistemas de facturación donde los mensajes duplicados o perdidos podrían conducir a cargos o cobros incorrectos.

•	Overhead pequeño e intercambio de protocolo minimizado para reducir la carga de trafico de red.
•	Un mecanismo para notificar a las partes interesadas cuando se produce una desconexión anormal

2.4.1.8.	Definiciones importantes

2.4.1.9.	Mensaje de aplicación

Son los datos transportados por el protocolo MQTT a través de la red. Cuando los mensajes de aplicación son transportados por MQTT tienen asociados un QoS y un nombre de tema o 'topic'

2.4.1.10.	Cliente

Es un programa o dispositivo que usa MQTT. Un cliente siempre establece una conexión de red y puede:

Publicar mensajes de aplicación que otros clientes pueden estar interesados en el.
Suscribirse para solicitar un mensaje de aplicación en el que se está interesado en recibir.
Cancelar la suscripción para retirar una solicitud de mensajes de aplicación.
Desconectarse del servidor.

2.4.1.11.	Servidor

Es un programa o dispositivo que actúa como intermediario entre clientes que publican mensajes de aplicación y clientes que han hecho subscripciones, un Servidor puede:

•	Aceptar conexiones de red desde los clientes
•	Aceptar mensajes de aplicación publicados por los clientes.
•	Procesar solicitudes de Subscribe y Unsubscribe desde los clientes.
•	Reenviar mensajes de aplicación que coinciden con las subscripciones de clientes.

2.4.1.12.	Suscripción

Una suscripción comprende un filtro de tema (topic filter) y un QoS. Una suscripción se asocia con una sola sesión. Una sesión puede contener más de una suscripción. Cada suscripción dentro de una sesión tiene un filtro de tema diferente.

2.4.1.13.	Topic Name:

Es la etiqueta adjunta a un mensaje de aplicación que se compara con las suscripciones conocidas por el servidor. El servidor envía una copia del mensaje de aplicación para cada cliente que tiene una suscripción coincidente.

2.4.1.14.	Topic Filter:

Es una expresión contenida en una suscripción, para indicar interés en uno a o mas tópicos. Un Topic Filter puede incluir caracteres comodín o wildcards.


2.4.1.15.	Session:

Es una interacción completa entre un cliente y un servidor. Algunas sesiones duran mientras hay conexión de red otras pueden abarcar múltiples consecutivas conexiones entre un cliente y un servidor.

2.4.1.16.	MQTT Control Packet

Es un paquete de información que se envía a través de la conexión de red. La especificación de MQTT define 14 tipos diferentes de paquetes de control, uno de los cuales (el paquete PUBLISH) se utiliza para transmitir mensajes de aplicación.
Los puertos TCP estándar de MQTT son el 1883 y 8883 para el caso de usar soporte para SSL/TLS,
MQTT fue inventado por Dr Andy Stanford-Clark de IBM y Arlen Nipper de Arcom en el año 1999

2.4.2.	Principio de diseño

•	Mensajeria Publish/Subscribe
•	Construido para situaciones de bajo BW, alta latencia o redes no confiables
•	Diseñado para dispositivos que puedan tener recursos limitados de procesamiento
