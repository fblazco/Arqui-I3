
## Jerarquia de Memoria y Cache
### Principios de localidad:
+ Principio de localidad temporal:
	+ Es probable que el dato obtenido en mem. sea utilizado posteriormente en otra operacion
+ Principio de localidad espacial:
	+ Es probable que datos cercanos al buscado tambien sean usados
### Jerarquia de Memoria:
-  La memoria se divide en distintos niveles, ordenados jerarquicamente segun la cercania a la CPU, mientras mas cerca mas rapido el acceso a la memoria.
- El nivel de memoria mas cercana a la CPU es la cache
### Memoria Cache:
- Controlada por un controlador de cache
- Dividida en lineas de memoria asociadas a bloques de memoria
- Controlador de cache:
	Funciones principales.
	- Mecanismo de acceso a datos: 
		1. Asocia bloques de memoria a lineas de cache
		2. Funcion de correspondencia entre linea de cache y bloque de memoria
		3. Politica de remplazo, se sobrescribe una linea de cache en caso de que no haya disp. en la copia de datos utilizando el principio de locaclidad temporal
	+ Politica de escritura: Como el controlador actualiza un dato en la mem. principal si la CPU realiza una escritura
### Funciones de correspondencia:
  + La formula de asociasion entre bloque de memoria y linea de cache depende de los siguientes factores.		  
	  1. Tamaño de linea de cache
	  2. Cantidad de lineaas de caceh
	  3. Cantidad de direcciones de la linea principal
	  4. Tag-> identificador unico para corroborar que el dato de una direccion de memoria princiapl se encuentra almacenado en la cache
  + **Directly Mapped**
	  + Cada bloque de la mem. principal se asocia solo a una linea de la cache 
	  + Cada linea de la cache poseera su indice 
	  + Num linea = Num bloque mod \# Lineas
	  + Ventajas: 
		  + Computo sencillo y directo
	+ Desventajas:
		+ Retencion entre bloques y lineas
		+ Desaprovechamiento del resto dela cache
+ **Fully Asociative**
	+ Cada bloque de la memoria se puede asociar a cualquier linea de la cache
	+ Aprovecha toda la memoria, maximiza el hit rate
	+ Complejiza el tag
	+ Desventajas:
		+ El hit time aumenta considerablemente por que se tiene que buscar coincidencia de tag en todas las lineas
		+ El almacenamiento del tag crece sustancialmente
+ **N-way Asociative**
	+ Cada bloque de memoria se asocia a un conjunto de N lineas de cache
	+ Divide la cache en conjuntos de N lineas
	+ Ventajas:
		+ Cada bloque posee un mapeo directo a su conjunto
		+ Dentro del conjunto el bloque se puede asociar a cualquier linea
	+ Ventajas:
		+ Hit time menor respecto a fully- associative
		+ Mayor uso de cache que directly mapped
		+ Ocupa menos bits de tag
	+ Desventajas:
		+ Hit time menor que directly mapped
		+ Menor uso de cache que fully associative
			(esta entre medio de ambas por eso tiene lo mejor de ambos mundos)		
+ **Remplazo de lineas:**
	 + Politicas de remplazo:
		+ Belady-> Se saca el bloque que se utilizara mas lejos en el futuro
		+ FIFO -> El primer bloque en entrar es el primero en salir
		+ LFU (least frequently used) -> El bloque con menos accesos se saca. 
		+ LRU (leas recently used) -> El bloque con mayor tiempo sin accesos se saca
		+ Random -> rapido, mejor que FIFO
### Politicas de Escritura:		
1. Write-through: 
	- bloque modificado se escribe directamente en mem. principal
	- menos conveniente por el tiempo de cada instruccion de escritura
3. Write Back: 
	- bloque modificado se escribe en la memoria principal solo cuando su linea en cache sera sustituida
	- mejora los tiempos pero genera problemas de sincronizacion
### Memoria Cache - Tipos de memoria:
1. Cache Unified
	+ Almacena datos e instrucciones
	+ Cuenta con la desventaja de su hit rate 
2. Cache Split
	+ Cache con division interna para datos e instrucciones
	+ Mayor hit rate pero hardware mas complejo