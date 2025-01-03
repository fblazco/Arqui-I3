### Coherencia de Cache

### Sistemas multiprocesador con memoria compartida

+ los procesadores comparten en algun nivel su memoria para facilitar la transferencia de datos
+ **Esquemas de memoria segun tipo de acceso**

  + Uniform Memory Access (UMA): los procesadores acceden a traveez de un buss o un crossbar switch a la memoria
  + Non-Uniform Memory Access (NUMA): Los procesadores se reparten nodos donde cada uno puede acceder a un segmento especifico de la memoria
+ **Memoria Compartida**

  + Tando en UMA como NUMA podemos hacer uso de memorias de cache para agilizar los accesos
  + Pero cada escritura implica un overhead que empeora el t de ejecucion
  + Nace el problema de la coherencia de cache
+ **Coherencia de cache**

  + Consistencia en la mem. de cache de 2 o mas procesadores que escriben o leen de una misma dir. de memoria.
  + Debemos asegurarnos que des pues de una escritura en mem. los otros procesadores siempre accedan a la version mas actualizada del dato
  + Para implementar eso usamos el ***controlador de cache***
+ **Controlador de Cache**

  + Bus Snooping
    + Consiste en la revision de las solicitudes de "R" y "W" que hacen los controladores de cache
  + Snarfing
    + Cuando un controlador de cache detecta una solicitud de "W" actualiza inmediatamente el contenido de su cache.
    + Es menos usada por el aumento de latencia y el aumento de trafico en el bus compartido de datos
+ **Protocolo MSI**

  + Protocolo que se basa en la adicion de estados para validar las lineas de cache, existen 3 estados posibles:
    + Modified
      + Indica que el contenido no es consisstente con el bloque de la mem. principal
    + Shared
      + Indica que el contenido de la linea se en cuantra en al menos una memoria de cache pero no se ha modificado
    + Invalid
      + Indica que el contenido de la linea es invalido
  + Eventos del MSI:
    + Solicitudes del proce actual:
      + PrRd: Solicitud de lectura (Read - Rd)
      + PrWe: Solicitud de escritura (Write - Wr)
    + Solicitudes del bus compartido:
      + BusRd: Solicitud de un bloque de datos sin intencion de modificacion
      + BusRdX: Solicitud de un bloque de datos con intencion de modificacion
      + Flush: Transferencia de linea de cache a la memoria principal
  + Cambios de estado de linea cache por eventos del procesador
    + Rellenar(...)
+ **Protocolo MESSI (lo mas grande)**
	+ Mejor que MSI, el estado Shared cambia su uso y se agrega un nuevo estado
	+ Shared
		+ Indica que el contenido se encuentre en almenos 2 caches
	+ Exclusive 
		+ Indica que el contenido de la linea se encuentra sin modificaciones y exclusivamente en dicha cache
	+ BusRd(S)
		+ Indica que se realizo una lectura y se obtuvo una confirmacion de la existencia del contenido en otra cache
	+ BusRd(not(S))
		+ Indica que se realiza una lectura y se obtuvo una confirmacion negativa de la existencia del contenido en otra cache (por descarte existe en la cache actual)
	+ **Cambios en estado de linea de cache por eventos del proce**
		+ La mayoria de transiciones queda igual
		+ Si se lee un dato que no esta en memoria y no esta en ninguna otra cache, el estado pasa a ser *Exclusive*. Si se realiza una escritura el estado cambia a *Modified*.
	+ **Cambios de estado de linea de cache por eventos del bus**
		+ La mayoria de transiciones queda igual
		+ Si un controlador recibe la señal BusRd para una linea en estado *Exclusive* entonces ya no es unica y cambia a estado *Modified*.
		+ Si un controlador recibe la señal BusRdX pasa a estado *Invalid*.

