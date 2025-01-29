Introducción
============

Conceptos básicos de Sistemas Operativos
----------------------------------------

Cada computadora incluye un conjunto básico de programas llamado *sistema operativo*. El programa más importante del conjunto se llama *kernel* o *kernel*. Se carga en la RAM cuando se inicia el sistema y contiene muchos procedimientos críticos que son necesarios para que el sistema funcione. Los otros programas son utilidades menos críticos; pueden proporcionar una amplia variedad de experiencias interactivas para el usuario, así como realizar todos los trabajos para los que el usuario compró la computadora, pero la forma y las capacidades esenciales del sistema están determinadas por el kernel. El kernel proporciona funciones clave para todo lo demás en el sistema y determina muchas de las características del software superior. Por lo tanto, a menudo usamos el término "sistema operativo" como sinónimo de "kernel".

El sistema operativo debe cumplir dos objetivos principales:

- Interactuar con los componentes de hardware, dando servicio a todos los elementos programables de bajo nivel incluidos en la plataforma de hardware.

- Proporcionar un entorno de ejecución para las aplicaciones que se ejecutan en el sistema informático (los llamados programas de usuario).

Algunos sistemas operativos permiten que todos los programas de usuario jueguen directamente con los componentes de hardware (un ejemplo típico es MS-DOS). Por el contrario, un sistema operativo tipo Unix oculta todos los detalles de bajo nivel relativos a la organización física de la computadora a las aplicaciones ejecutadas por el usuario. Cuando un programa desea utilizar un recurso de hardware, debe enviar una solicitud al sistema operativo. El kernel evalúa la solicitud y, si decide conceder el recurso, interactúa con los componentes de hardware adecuados en nombre del programa del usuario.

Para hacer cumplir este mecanismo, los sistemas operativos modernos dependen de la disponibilidad de características específicas del hardware que prohíben a los programas de usuario interactuar directamente con componentes de hardware de bajo nivel o acceder a ubicaciones de memoria arbitrarias. En particular, el hardware introduce al menos dos *modos de ejecución* diferentes para la CPU: un modo sin privilegios para los programas de usuario y un modo privilegiado para el kernel. Unix los llama *modo usuario* y *modo kernel*, respectivamente.

Sistemas multiusuarios
----------------------

Un *sistema multiusuario* es un ordenador que puede ejecutar de forma simultánea e independiente varias aplicaciones pertenecientes a dos o más usuarios. *Concurrentemente* significa que las aplicaciones pueden estar activas al mismo tiempo y competir por los distintos recursos, como CPU, memoria, discos duros, etc. *Independientemente* significa que cada aplicación puede realizar su tarea sin preocuparse por lo que estén haciendo las aplicaciones de los demás usuarios. Cambiar de una aplicación a otra, por supuesto, ralentiza cada una de ellas y afecta el tiempo de respuesta que ven los usuarios. Muchas de las complejidades de los kernels de los sistemas operativos modernos, están presentes para minimizar los retrasos impuestos a cada programa y para proporcionar al usuario respuestas lo más rápidas posibles.

Los sistemas operativos multiusuario deben incluir varias características:

- Un mecanismo de autenticación para verificar la identidad del usuario
- Un mecanismo de protección contra programas de usuario con errores que podrían bloquear otras aplicaciones que se ejecutan en el sistema
- Un mecanismo de protección contra programas de usuario maliciosos que podrían interferir o espiar la actividad de otros usuarios
- Un mecanismo de registros que limite la cantidad de unidades de recursos asignadas a cada usuario 

Para garantizar mecanismos de protección seguros, los sistemas operativos deben utilizar la protección de hardware asociada con el modo privilegiado de la CPU. De lo contrario, un programa de usuario podría acceder directamente a los circuitos del sistema y superar los límites impuestos. Unix es un sistema multiusuario que aplica la protección de hardware de los recursos del sistema.

Usuarios y Grupos
-----------------

En un sistema multiusuario, cada usuario tiene un espacio privado en la máquina; normalmente, posee una cuota de espacio en disco para almacenar archivos, recibir mensajes de correo privados, etc. El sistema operativo debe garantizar que la parte privada de un espacio de usuario sea visible sólo para su propietario. En particular, debe garantizar que ningún usuario pueda explotar una aplicación del sistema con el fin de violar el espacio privado de otro usuario.

Todos los usuarios se identifican mediante un número único llamado *ID de usuario* o *UID*. Por lo general, sólo se permite el uso de un sistema informático a un número restringido de personas. Cuando uno de estos usuarios inicia una sesión de trabajo, el sistema solicita un *nombre de usuario* de sesión y una *contraseña*. Si el usuario no introduce un par válido, el sistema niega el acceso. Como se supone que la contraseña es secreta, se garantiza la privacidad del usuario.

Para compartir material de forma selectiva con otros usuarios, cada usuario es miembro de uno o más *grupos de usuarios*, que se identifican mediante un número único llamado *ID de grupo de usuarios*. Cada archivo está asociado exactamente a un grupo. Por ejemplo, el acceso puede configurarse de modo que el usuario propietario del archivo tenga privilegios de lectura y escritura, el grupo tenga privilegios de sólo lectura y a los demás usuarios del sistema se les niegue el acceso al archivo.

Cualquier sistema operativo tipo Unix tiene un usuario especial llamado *root* o *superusuario*. El administrador del sistema debe iniciar sesión como root para gestionar las cuentas de usuario, realizar tareas de mantenimiento como copias de seguridad del sistema y actualizaciones de programas, etc. El usuario root puede hacer casi todo, porque el sistema operativo no le aplica los mecanismos de protección habituales. En particular, el usuario root puede acceder a todos los archivos del sistema y puede manipular todos los programas de usuario en ejecución.

Procesos
--------

Todos los sistemas operativos utilizan una abstracción fundamental: el *proceso*. Un proceso puede definirse como “una instancia de un programa en ejecución” o como el “contexto de ejecución” de un programa en ejecución. En los sistemas operativos tradicionales, un proceso ejecuta una única secuencia de instrucciones en un *espacio de direcciones*; el espacio de direcciones es el conjunto de direcciones de memoria a las que el proceso puede hacer referencia. Los sistemas operativos modernos permiten procesos con múltiples flujos de ejecución, es decir, múltiples secuencias de instrucciones ejecutadas en el mismo espacio de direcciones.

Los sistemas multiusuario deben aplicar un entorno de ejecución en el que varios procesos puedan estar activos simultáneamente y competir por los recursos del sistema, principalmente la CPU. Los sistemas que permiten procesos activos simultáneos se denominan *multiprogramados* o *multiprocesamiento*. [#]_ Es importante distinguir los programas de los procesos; varios procesos pueden ejecutar el mismo programa simultáneamente, mientras que el mismo proceso puede ejecutar varios programas secuencialmente.

En los sistemas monoprocesador, solo un proceso puede contener la CPU y, por lo tanto, solo puede progresar un flujo de ejecución a la vez. En general, el número de CPUs siempre está restringido y, por lo tanto, sólo unos pocos procesos pueden avanzar a la vez. Un componente del sistema operativo llamado *planificador* elige el proceso que puede avanzar. Algunos sistemas operativos sólo permiten procesos *no preemptivos*, lo que significa que el planificador se invoca sólo cuando un proceso renuncia voluntariamente a la CPU. Pero los procesos de un sistema multiusuario deben ser preemptivos; el sistema operativo rastrea cuánto tiempo cada proceso retiene la CPU y activa periódicamente el planificador.

Unix es un sistema operativo multiprocesamiento con procesos preemptivos. Incluso cuando ningún usuario está conectado y ninguna aplicación está ejecutándose, varios procesos del sistema monitorean los dispositivos periféricos. En particular, varios procesos escuchan en las terminales del sistema esperando que los usuarios inicien sesión. Cuando un usuario ingresa un nombre de inicio de sesión, el proceso que escucha ejecuta un programa que valida la contraseña del usuario. Si se reconoce la identidad del usuario, el proceso crea otro proceso que ejecuta un shell en el que se ingresan comandos. Cuando se activa una pantalla gráfica, un proceso ejecuta el administrador de ventanas y cada ventana en la pantalla generalmente es ejecutada por un proceso separado. Cuando un usuario crea un shell gráfico, un proceso ejecuta las ventanas gráficas y un segundo proceso ejecuta el shell en el que el usuario puede introducir los comandos. Para cada comando del usuario, el proceso del shell crea otro proceso que ejecuta el programa correspondiente.

Los sistemas operativos tipo Unix adoptan un *modelo de proceso/kernel*. Cada proceso tiene la ilusión de que es el único proceso en la máquina y tiene acceso exclusivo a los servicios del sistema operativo. Siempre que un proceso realiza una llamada al sistema (es decir, una solicitud al kernel, consulte llamadas_ al sistema), el hardware cambia el modo de privilegio del Modo Usuario al Modo kernel y el proceso inicia la ejecución de un procedimiento del kernel con un propósito estrictamente limitado. De esta manera, el sistema operativo actúa dentro del contexto de ejecución del proceso para satisfacer su solicitud. Siempre que la solicitud se satisface por completo, el procedimiento del kernel obliga al hardware a volver al Modo Usuario y el proceso continúa su ejecución a partir de la instrucción que sigue a la llamada al sistema.


Arquitectura del Kernel
-----------------------

Como se ha dicho antes, la mayoría de los kernels Unix son monolíticos: cada capa del kernel está integrada en el programa del kernel y se ejecuta en modo kernel en nombre del proceso actual. Por el contrario, los sistemas operativos *microkernel* exigen un conjunto muy pequeño de funciones del kernel, que generalmente incluyen unas pocas primitivas de sincronización, un planificador simple y un mecanismo de comunicación entre procesos. Varios procesos del sistema que se ejecutan sobre el microkernel implementan otras funciones de la capa del sistema operativo, como asignadores de memoria, controladores de dispositivos y manejadores de llamadas del sistema.

Aunque la investigación académica sobre sistemas operativos está orientada hacia los microkernels, estos sistemas operativos son generalmente más lentos que los monolíticos, porque el uso de mensajes entre las diferentes capas del sistema operativo tiene un costo. Sin embargo, los sistemas operativos con microkernel pueden tener algunas ventajas teóricas sobre los monolíticos. Los microkernels obligan a los programadores de sistemas a adoptar un enfoque modularizado, porque cada capa del sistema operativo es un programa relativamente independiente que debe interactuar con las otras capas a través de interfaces de software bien definidas y claras. Además, un sistema operativo con microkernel existente puede ser fácilmente portado a otras arquitecturas con bastante facilidad, porque todos los componentes dependientes del hardware están generalmente encapsulados en el código del microkernel. Finalmente, los sistemas operativos con microkernel tienden a hacer un mejor uso de la memoria de acceso aleatorio (RAM) que los monolíticos, porque los procesos del sistema que no están implementando las funcionalidades necesarias pueden ser intercambiados o destruidos.

Para lograr muchas de las ventajas teóricas de los microkernels sin introducir penalizaciones de rendimiento, el kernel de Linux ofrece *módulos*. Un módulo es un archivo objeto cuyo código puede ser vinculado al kernel (y desvinculado del mismo) en tiempo de ejecución. El código objeto generalmente consiste en un conjunto de funciones que implementan un sistema de archivos, un controlador de dispositivo u otras características en la capa superior del kernel. El módulo, a diferencia de las capas externas de los sistemas operativos con microkernel, no se ejecuta como un proceso específico. En cambio, se ejecuta en modo kernel en nombre del proceso actual, como cualquier otra función del kernel vinculada estáticamente.

..  image:: ..images/imagen1.png

Las principales ventajas de utilizar módulos incluyen:

*Un enfoque modularizado*

    Debido a que cualquier módulo puede ser vinculado y desvinculado en tiempo de ejecución, los programadores de sistemas deben introducir interfaces de software bien definidas para acceder a las estructuras de datos manejadas por los módulos. Esto facilita el desarrollo de nuevos módulos.

*Independencia de la plataforma*

    Incluso si puede depender de algunas características de hardware específicas, un módulo no depende de una plataforma de hardware fija. Por ejemplo, un módulo de controlador de disco que se basa en el estándar SCSI funciona tan bien en una PC compatible con IBM como en Alpha de Hewlett-Packard.

*Uso moderado de la memoria principal*

    Un módulo puede vincularse al kernel en ejecución cuando se requiere su funcionalidad y     desvincularse cuando ya no es útil; esto es bastante útil para pequeños sistemas integrados.

*Sin penalización de rendimiento*

    Una vez vinculado, el código objeto de un módulo es equivalente al código objeto del kernel vinculado estáticamente. Por lo tanto, no se requiere el paso explícito de mensajes cuando se invocan las funciones del módulo. [#]_

..  [#] Algunos sistemas operativos multiprocesamiento no son multiusuarios.
..  [#] Se produce una pequeña pérdida de rendimiento cuando se vincula y desvincula el módulo. Sin embargo, esta pérdida se puede comparar con la pérdida causada por la creación y eliminación de procesos del sistema en sistemas operativos con microkernel.

..  _llamadas: Ver referencia
