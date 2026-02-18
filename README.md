# Laboratorio #4: Introducción a Esquemas de Nombres, Redes, Clientes y Servicios con Java

## Autores
- Carolina Cepeda Valencia
- Deisy Lorena Guzman Cabrales
- Jared Sebastian Farfan Guevara


## Tabla de Contenidos
1. [Introducción](#introducción)
2. [Objetivos](#objetivos)
3. [Marco Teórico](#marco-teórico)
4. [Desarrollo de Ejercicios](#desarrollo-de-ejercicios)
   - [Punto 5b: Datagramas UDP](#punto-5b-datagramas-udp---servidor-de-tiempo)
5. [Resultados](#resultados)
6. [Conclusiones](#conclusiones)
7. [Referencias](#referencias)

## Introducción

La programación de aplicaciones distribuidas es fundamental en el desarrollo de software moderno. La comunicación entre procesos remotos permite crear sistemas escalables, distribuidos y eficientes. Este laboratorio se enfoca en el estudio y la implementación práctica de diversos mecanismos de comunicación en red utilizando el lenguaje de programación Java.

A lo largo de este taller, se exploran diferentes paradigmas y protocolos de comunicación, desde el uso básico de URLs para obtener recursos de internet, hasta la implementación de sistemas cliente-servidor utilizando sockets TCP, servidores web HTTP, datagramas UDP, y finalmente la invocación remota de métodos (RMI).

Este documento presenta el desarrollo sistemático de siete ejercicios prácticos que abordan progresivamente conceptos fundamentales de redes de computadoras, comenzando con operaciones de lectura de URLs, pasando por la implementación de clientes y servidores con diferentes protocolos, hasta culminar con un sistema de chat distribuido usando RMI.

## Objetivos

### Objetivo General
Comprender y aplicar los conceptos fundamentales de comunicación en red utilizando Java, implementando soluciones prácticas que demuestren el uso de diferentes protocolos y tecnologías de comunicación distribuida.

### Objetivos Específicos
1. Comprender el manejo de URLs en Java y sus métodos para extraer información de direcciones web.
2. Implementar clientes que se conecten a servicios remotos y almacenen información localmente.
3. Desarrollar sistemas cliente-servidor utilizando sockets TCP para comunicación bidireccional.
4. Crear servidores con capacidad de procesamiento de datos y cambio dinámico de funcionalidad.
5. Implementar un servidor web HTTP capaz de servir múltiples tipos de recursos.
6. Aplicar el protocolo UDP mediante datagramas para comunicación sin conexión y desarrollar clientes resilientes.
7. Aplicar el paradigma de invocación remota de métodos (RMI) para sistemas distribuidos.

## Marco Teórico

### Conceptos Básicos de Redes

#### TCP (Transmission Control Protocol)
El Transmission Control Protocol es un protocolo orientado a conexión que garantiza la entrega confiable y ordenada de datos entre dos sistemas. TCP establece una conexión antes de transmitir datos y mantiene el estado de la comunicación durante toda la sesión. Este protocolo es ideal para aplicaciones que requieren integridad de datos, como transferencia de archivos, correo electrónico y páginas web.

#### UDP (User Datagram Protocol)
El User Datagram Protocol es un protocolo sin conexión que envía datagramas de manera independiente sin garantizar su entrega ni orden. UDP es más rápido que TCP pero menos confiable. Se utiliza en aplicaciones donde la velocidad es más importante que la fiabilidad, como streaming de video, juegos en línea y servicios de tiempo.

#### Puertos
Los puertos son números lógicos de 16 bits (rango 0-65535) que permiten a un sistema operativo dirigir el tráfico de red a aplicaciones específicas. Los puertos del 0 al 1023 están reservados para servicios conocidos (por ejemplo, puerto 80 para HTTP, puerto 443 para HTTPS).

### URLs (Uniform Resource Locator)
Una URL es una referencia a un recurso web que especifica su ubicación en una red. La estructura general de una URL es: `<protocolo>://<servidor>:<puerto>/<ruta>`

### Sockets
Los sockets son abstracciones de software que representan los puntos finales de una conexión de red bidireccional. En Java, las clases `Socket` (cliente) y `ServerSocket` (servidor) del paquete `java.net` facilitan la comunicación TCP.

### RMI (Remote Method Invocation)
RMI es un mecanismo que permite a un objeto Java invocar métodos de otro objeto que reside en una máquina virtual diferente. RMI proporciona transparencia de ubicación, permitiendo que el código cliente llame a métodos remotos como si fueran locales.

## Desarrollo de Ejercicios

### Punto 1: URLReader - Lectura y Análisis de URLs

**Descripción:** Implementación de un programa que crea un objeto URL e imprime los valores retornados por los métodos `getProtocol()`, `getAuthority()`, `getHost()`, `getPort()`, `getPath()`, `getQuery()`, `getFile()` y `getRef()`.

**Implementación:**
El programa `URLReader.java` solicita al usuario una dirección URL y muestra cada uno de sus componentes de forma estructurada utilizando los métodos disponibles en la clase URL de Java para extraer el protocolo, autoridad, host, puerto, ruta, query, archivo y referencia.

**Características:**
- Validación de formato de URL mediante manejo de excepciones
- Interfaz interactiva que permite ingresar múltiples URLs
- Descomposición completa de la estructura de una URL

### Punto 2: Browser - Descarga de Contenido Web

**Descripción:** Aplicación que solicita una URL al usuario, lee el contenido de esa dirección y lo almacena en un archivo `resultado.html`, el cual puede ser visualizado posteriormente en un navegador web.

**Implementación:**
El programa `Browser.java` establece una conexión HTTP utilizando la clase URL, lee el flujo de datos mediante un BufferedReader y lo escribe línea por línea en un archivo local llamado `resultado.html` usando un PrintWriter. La implementación utiliza try-with-resources para garantizar el cierre adecuado de los recursos.

**Características:**
- Uso de streams para lectura eficiente de datos
- Implementación de try-with-resources para manejo automático de recursos
- Almacenamiento persistente del contenido descargado

### Punto 3: SquareClient y SquareServer - Servidor de Cálculo de Cuadrados

**Descripción:** Sistema cliente-servidor donde el servidor recibe números del cliente y responde con el cuadrado de dichos números, utilizando comunicación TCP mediante sockets.

**Implementación del Servidor:**
`SquareServer.java` escucha en el puerto 35000 y procesa las solicitudes recibidas. Lee cada línea enviada por el cliente, intenta parsearla como un número double, calcula su cuadrado y envía la respuesta. Incluye manejo de excepciones para entradas inválidas.

**Implementación del Cliente:**
`SquareClient.java` establece conexión con el servidor en localhost puerto 35000, lee números desde la entrada estándar, los envía al servidor y muestra las respuestas recibidas.

**Características:**
- Validación de entrada numérica con manejo de excepciones
- Protocolo de terminación mediante palabra clave "Bye."
- Comunicación sincrónica bidireccional

### Punto 4: FunClient y FunServer - Servidor de Funciones Trigonométricas

**Descripción:** Servidor que calcula funciones trigonométricas (seno, coseno, tangente) sobre números recibidos. El servidor puede cambiar dinámicamente la función a calcular mediante comandos especiales que comienzan con "fun:".

**Implementación del Servidor:**
`FunServer.java` implementa un sistema de cambio dinámico de funciones trigonométricas. El servidor mantiene un estado interno que almacena la función actual (inicialmente coseno). Cuando recibe un mensaje que comienza con "fun:", extrae el nombre de la función y la cambia si es válida (sin, cos, tan). Para entrada numérica, calcula el resultado de aplicar la función actual sobre el número usando la clase Math de Java.

**Características:**
- Soporte para múltiples clientes secuenciales
- Cambio dinámico de función mediante protocolo de comandos
- Uso de la clase `Math` para cálculos trigonométricos precisos
- Estado interno que mantiene la función actual

**Ejemplo de uso:**
Al enviar el valor 0, el servidor responde 1.0 (coseno de 0). Al enviar "fun:sin", el servidor cambia a función seno. Al enviar 1.5707963 (π/2), el servidor responde 1.0 (seno de π/2).

### Punto 5: ServidorWeb - Servidor HTTP Multipropósito

**Descripción:** Servidor web que atiende múltiples solicitudes HTTP consecutivas y sirve diferentes tipos de archivos (HTML, imágenes, CSS, JavaScript) desde el sistema de archivos local.

**Implementación:**
`ServidorWeb.java` implementa un servidor HTTP básico que parsea las solicitudes HTTP, extrae el nombre del archivo solicitado, verifica su existencia en el sistema de archivos, determina el tipo MIME apropiado según la extensión del archivo, y envía la respuesta HTTP con las cabeceras correctas. Si el archivo no existe, retorna una respuesta 404 con un mensaje HTML.

**Características:**
- Soporte para múltiples tipos MIME (HTML, imágenes, CSS, JavaScript)
- Manejo de códigos de estado HTTP (200 OK, 404 Not Found)
- Cabeceras HTTP correctas (Content-Type, Content-Length)
- Ciclo infinito para atender múltiples solicitudes consecutivas
- Servicio de archivos desde directorio local

**Recursos servidos:**
- `escuelaing.html` - Página HTML de ejemplo
- `google.html` - Página HTML de ejemplo

**Acceso:**
El servidor escucha en el puerto 35000 y los recursos pueden ser accedidos mediante un navegador web usando localhost.

### Punto 5b: Datagramas UDP - Servidor de Tiempo

**Descripción:** Sistema cliente-servidor que utiliza el protocolo UDP (User Datagram Protocol) para transmitir información de tiempo. El servidor responde con la hora actual cada vez que recibe una solicitud, y el cliente consulta periódicamente actualizando la hora cada 5 segundos.

**Implementación del Servidor:**
`DatagramTimeServer.java` crea un DatagramSocket escuchando en el puerto 4445. En un ciclo infinito, espera por paquetes UDP entrantes, obtiene la hora actual del sistema, convierte la fecha a bytes y la envía de vuelta a la dirección y puerto del cliente que hizo la solicitud. El servidor mantiene operación continua para atender múltiples solicitudes.

**Implementación del Cliente:**
`DatagramTimeClient.java` implementa un cliente resiliente que consulta el servidor cada 5 segundos. Crea un DatagramSocket con timeout de 6 segundos, envía un datagrama vacío al servidor en localhost:4445, y espera la respuesta. Si el servidor no responde (timeout), muestra "Servidor no disponible" pero mantiene la última hora conocida y continúa intentando. Si recibe respuesta, actualiza y muestra la hora actual.

**Características:**
- Protocolo UDP sin garantía de entrega
- Cliente con manejo de timeouts y reconexión automática
- Actualización periódica cada 5 segundos mediante Thread.sleep()
- Servidor stateless que responde a cada solicitud independientemente
- Resiliencia del cliente ante caídas temporales del servidor
- Uso de DatagramPacket para encapsular mensajes UDP

**Comportamiento ante fallos:**
El cliente está diseñado para sobrevivir a interrupciones del servidor. Si el servidor se detiene, el cliente muestra "Servidor no disponible" pero continúa ejecutándose y reintentando la conexión. Cuando el servidor se reactiva, el cliente automáticamente retoma la recepción de actualizaciones de hora sin necesidad de reiniciarse.

### Punto 6: ChatApp - Sistema de Chat Distribuido con RMI

**Descripción:** Aplicación de chat distribuida que utiliza Java RMI para permitir la comunicación bidireccional entre dos instancias de la aplicación ejecutándose en diferentes máquinas virtuales.

**Arquitectura:**
El sistema consta de tres componentes:

1. **ChatService.java** - Interfaz remota que extiende Remote y define el contrato de comunicación con un método para recibir mensajes que lanza RemoteException.

2. **ChatServiceImpl.java** - Implementación del servicio que crea un registry RMI en el puerto especificado, exporta el objeto usando UnicastRemoteObject, y lo publica en el registry con un nombre específico. Implementa el método para recibir mensajes que imprime en consola los mensajes recibidos.

3. **ChatApp.java** - Aplicación principal que actúa como cliente y servidor simultáneamente. Primero publica su propio servicio chat, luego solicita al usuario los datos del chat remoto (IP, puerto, nombre), se conecta al registry remoto, obtiene la referencia al servicio remoto mediante lookup, y en un ciclo lee mensajes del usuario y los envía invocando el método remoto.

**Características:**
- Arquitectura peer-to-peer donde cada instancia es cliente y servidor
- Uso de RMI Registry para publicación y descubrimiento de servicios
- Comunicación bidireccional sin servidor central
- Configuración dinámica de puertos y nombres de servicios
- Exportación de objetos mediante `UnicastRemoteObject`

**Flujo de comunicación:**
1. Cada usuario publica su servicio en un registry local
2. Un usuario se conecta al registry del otro usuario
3. Obtiene la referencia remota al objeto ChatService
4. Invoca el método `recibirMensaje()` como si fuera local
5. El mensaje se transmite y se muestra en la otra aplicación

## Resultados

### Punto 1: URLReader
El programa permite analizar exitosamente URLs complejas, extrayendo todos sus componentes. Por ejemplo, al ingresar `http://ldbn.escuelaing.edu.co:80/index.html?param=value#section`, se obtienen correctamente el protocolo (http), host (ldbn.escuelaing.edu.co), puerto (80), ruta (/index.html), query (param=value) y referencia (section).

### Punto 2: Browser
La aplicación descarga exitosamente el contenido HTML de URLs especificadas, creando un archivo `resultado.html` que puede ser visualizado en cualquier navegador web, preservando la estructura del documento original.

### Punto 3: SquareServer/Client
El sistema cliente-servidor funciona correctamente, calculando cuadrados de números con precisión. La validación de entrada previene errores de formato y el protocolo de terminación permite cerrar la conexión ordenadamente.

### Punto 4: FunServer/Client
El servidor trigonométrico cambia dinámicamente entre funciones según los comandos recibidos. Los cálculos son precisos y el sistema maneja correctamente valores especiales como π/2, 0, y π.

### Punto 5: ServidorWeb
El servidor web atiende múltiples solicitudes consecutivas, sirve correctamente archivos HTML e imágenes con los tipos MIME apropiados, y maneja adecuadamente recursos no encontrados con páginas 404 personalizadas.

### Punto 5b: Datagramas UDP
El sistema de tiempo mediante datagramas UDP funciona correctamente. El cliente actualiza la hora cada 5 segundos cuando el servidor está disponible. Durante las pruebas de resiliencia, al detener el servidor, el cliente muestra "Servidor no disponible" y mantiene la última hora conocida. Al reactivar el servidor, el cliente automáticamente retoma las actualizaciones sin requerir reinicio, demostrando la robustez del diseño frente a fallos de red.

### Punto 6: ChatApp
El sistema de chat distribuido permite comunicación efectiva entre dos instancias de la aplicación. Los mensajes se transmiten instantáneamente mediante RMI y el sistema es estable para sesiones de chat prolongadas.

## Conclusiones

1. **Protocolos de red:** Se comprendió exitosamente la diferencia fundamental entre TCP (orientado a conexión, confiable) y UDP (sin conexión, rápido), aplicando cada uno según las necesidades de la aplicación.

2. **Abstracción de URLs:** Java proporciona una API completa para manipular URLs, facilitando el acceso a recursos web y la descomposición de sus componentes de manera estructurada.

3. **Sockets TCP:** La implementación de sistemas cliente-servidor con sockets demostró ser robusta y adecuada para comunicaciones que requieren garantía de entrega y orden.

4. **Protocolos personalizados:** Es posible diseñar protocolos de comunicación específicos (como el cambio de funciones con "fun:") para necesidades particulares de la aplicación.

5. **Servidor HTTP:** Implementar un servidor web básico ayuda a comprender el funcionamiento interno del protocolo HTTP, incluyendo códigos de estado, cabeceras y tipos MIME.

6. **Datagramas UDP:** La implementación con datagramas demostró las características del protocolo UDP: velocidad y simplicidad a cambio de no garantizar entrega. El diseño de clientes resilientes con manejo de timeouts es esencial para aplicaciones UDP robustas.

7. **RMI:** La invocación remota de métodos proporciona un nivel de abstracción superior, permitiendo programar aplicaciones distribuidas con sintaxis orientada a objetos, ocultando la complejidad de la comunicación en red.

8. **Manejo de recursos:** El uso de try-with-resources y el cierre apropiado de conexiones son críticos para evitar fugas de recursos en aplicaciones de red.

9. **Escalabilidad:** Las implementaciones actuales son secuenciales; una mejora importante sería el uso de multithreading para atender múltiples clientes concurrentemente.

10. **Seguridad:** Las implementaciones básicas no incluyen autenticación, encriptación ni validación exhaustiva. En aplicaciones reales, estos aspectos son fundamentales.

11. **Paradigmas de comunicación:** Se exploraron exitosamente cuatro paradigmas: cliente-servidor tradicional con TCP (sockets), comunicación sin conexión con UDP (datagramas), petición-respuesta (HTTP), y peer-to-peer con RMI, cada uno con ventajas específicas según el caso de uso.

