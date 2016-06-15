# MQTT

###Protocolo MQTT - (Message Queueing Telemetry Transport)

MQTT (Message Queuing Telemetry Transport) es un protocolo de conectividad para sistemas M2M, su funcionamiento se basa en mensajes publish/subscribe extremadamente simple y liviano, fue diseñado para dispositivos que funcionan con restricciones de ancho de banda, alta latencia o redes poco confiables, dado lo anterior dentro de sus caracteristicas implementa un cierto grado de garantia en la entrega de los mensajes.

El protocolo trabaja sobre TCP/IP e incluye las siguientes caracteristicas:

- Transporte de mensajes que es indepediente al contenido de la carga util o payload.
- Tres tipos de QoS para entrega de mensajes
  * 'At most once' o QoS = 0, donde los mensajes son entregados segun el mejor esfuerzo por lo tanto pueden darse situaciones de perdidas de mensajes. Este nivel puede ser usado por ejemplo; en situaciones en que una lectura individual de datos de un sensor de ambiente se pierde sabiendo que en la proxima se publicara poco despues.
  * 'At least onde' o QoS =1, donde se asegura que los mensajes llegaran a destino pero duplicados pueden ocurrir.
  * 'Exactly once' o QoS =3, donde se asegura que los mensajes llegaran a destino exactamente 1 vez. Este nivel podría ser utilizado por ejemplo, con sistemas de facturación donde los mensajes duplicados o perdidos podrían conducir a cargos o cobros incorrectos.
- Overhead pequeño e intercambio de protocolo minimizado para reducir la carga de trafico de red.
- Un mecanismo para notificar a las partes interesadas cuando se produce una desconexión anormal

####Definiciones importantes

- Mensaje de aplicacion:

  Son los datos transportador por el protocolo MQTT a travez de la red. Cuando los mensajes de aplicación son transportados por MQTT tienen asociados un QoS y un nombre de tema o 'topic'

- Cliente

  Es un programa o dispositivo que usa MQTT. Un cliente siempre establece una conexion de red y puede:
  
   * Publicar mensajes de aplicación que otros clientes pueden estar interesados en el.
   * Suscribirse para solicitar un mensaje de aplicación en el que se está interesado en recibir.
   * Cancelar la suscripción para retirar una solicitud de mensajes de aplicación.
   * Desconectarse del servidor.
- Servidor

  Es un programa o dispositivo que actua como intermediario entre clientes que publican mensajes de aplicacion y clientes que han hecho subscripciones, un Servidor puede:
   
   * Aceptar conexiones de red desde los clientes
   * Aceptar mensajes de aplicacion publicados por los clientes.
   * Procesar solicitudes de Subscribe y Unsubscribe desde los clientes.
   * Reenviar mensajes de aplicacion que coinciden con las subscripciones de clientes.
- Subscripcion:

  Una suscripción comprende un filtro de tema (topic filter) y un QoS. Una suscripción se asocia con una sola sesión. Una sesión puede contener más de una suscripción. Cada suscripción dentro de una sesión tiene un filtro de tema diferente.

- Topic Name:

  Es la etiqueta adjunta a un mensaje de aplicación que se compara con las suscripciones conocidas por el servidor. El servidor envía una copia del mensaje de aplicacion para cada cliente que tiene una suscripción coincidente.

- Topic Filter:

  Es una expresion contenida en una subcripcion, para indicar interes en uno a o mas topicos. Un Topic Filter puede incluir caracteres comodin o wildcards.

- Session:

  Es una interaccion completa entre un cliente y un servidor. Algunas sesiones duran mientras hay conexion de red otras pueden abarcar multiples consecutivas conexiones entre un cliente y un servidor.

- MQTT Control Packet:

  Es un paquete de información que se envía a través de la conexión de red. La especificación de MQTT define 14 tipos diferentes de paquetes de control, uno de los cuales (el paquete PUBLISH) se utiliza para transmitir mensajes de aplicación.


Los puertos TCP estandar de MQTT son el 1883 y 8883 para el caso de usar soporte para SSL/TLS,

MQTT fue inventado por Dr Andy Stanford-Clark de IBM y Arlen Nipper de Arcom en el año 1999


####Principio de diseño

- Mensajeria Publish/Subscribe
- Construido para situaciones de bajo BW, alta latencia o redes no confiables
- Diseñado para dispositivos que puedan tener recursos limitados de procesamiento
