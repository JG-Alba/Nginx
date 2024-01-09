¿En que se diferencia?

Las diferencias entre apache y ngix residen en el rendimiento.

Apache necesita multiples hilos para atender a multiples peticiones del cliente, mientras que Nginx puede atender multiples con un solo hilo.

Apache no puede atender multiples peticiones de forma concurrente y asincrona, Nginx todo lo contrario

Apache puede usar php dentro del propio servidor, Nginx lo externaliza y lo hace mas eficiente.

A la hora de tener contenido estatico, Nginx es mucho mas eficiente.

Nginx funciona como proxy inverso mientras que Apache solo funciona como servidor web.

Algunas otras diferencias son:

Apache esta disponible en cualquier sistema operativos, mientras que Nginx funciona mal en windows.

Apache carga y descarga modulos facilmente, en Nginx hay que compilarlos.

Apache esta desarrollado por la comunidad mientras que nginx es de una empresa.