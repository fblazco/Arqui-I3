
##Multiprogramacion

Simulacion de procesamiento paralelo que lleva a cabo el computador, se basa en la ejecucion intercalada de los programas. Si bien no es 100% ejecucion paralela, para el usuario se percibe como paralelismo real.
+ Memoria Virtual:
	+  Espacio de memoria direccionable que se traduce al espacio direccionable de la memoria principal 
	+ MMU (memory management unit), encargada de traducir direcciones fisicas a mem. principal
	+  espacio mem. virtual > esp. mem. fisica, asi el pc puede ocupar mas espacio del base
+ Traduccion de Direcciones:
	+ La MMU tiene una tabla que funciona como diccionario asociando direcciones virtuales y fisicas
	+ Estas asociaciones no es tan en formato 1 a 1 si no que se usa el metodo de Paginacion
	+ Paginacion:
		+ La memoria virtual se divide en bloques contiguos llamados paginas
		+ Cada pagina se mapea a un marco de la memoria fisica (comparten tamaño)
		+ Usamos los bits de la direccion virtual para separar numero de pagina del offset de la palabra buscada en la pagina
+ Tabla de Paginas:
	+ Se encuentra en la seccion protegida de la memoria, almacenada en el registro especial PTBR (pete brasileño)
	+ El indice de cada entrada (PTE) representa el numero de pagina conteniendo los bit de marco fisico y los bits de metadata 
+ Bits de metadata: 
	 + Entregan informacion sobre la asociacion pagina marco fisico
	 + Nos interesa uno en particular, el present bit
	 + Present Bit: Indica si el contenido de la entrada de la tabla de paginas esta en un marco fisico o en el swap file
+ Page Fault:
	+ Ocurre si el contenido del marco fisico de una pagina esta en el swap file. 
	+ Solucion: swap in, copiar el contenido de la pagina desde el swap file en un marco disponible.
	+ swap out: si no hay marcos disponibles se libera uno y se realiza swap in nuevamente.
+ Paginas sin marco fisico:
	+ (...) Rellenar no entra igual
+ Accesos a memoria:
	+ Para obtener un dato de la memoria principal necesitamos por lo menos acceder 2 veces a esta.
	+ Un acceso para entrar a la tabala de paginas para traducir a una direccion fisica 
	+ Otro acceso a la jerarquia de memoria para el dato deseado
	+ Para solucionar esto agregamos otra memoria cache, la TLB (translation lookaside buffer)
		+ mem. pequeña
		+ guarda las traducciones de la pagina que van siendo accedidas
		+ fully associative y con niveles (L1, L2, L3)
		+ Almacena lo minimo para realizar la traduccion y requiere de un bit de validez
	+ Si se accede 2 veces a la TLB ocurre un cambio de conteto y las entradas de esta pasan a ser invalidas en el nuevo proceso soluciones:
		+ Flush -> Se vacia la cache (valid bit 0 para todas las entradas)
		+ ASID -> se agrega un identificador del proceso al que pertenece cada traduccion
	+ Una modificacion para disminuir el overhead del swapping es añadir un dirty bit, este se agrega a la metadata de cada PTE e indica si el marco fisico ha sido modificado o no
+ Tamaño de la tabla de paginas