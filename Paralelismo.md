## Paralelismo a Nv. de Instruccion (ILP)

### Paralelismo en la CPU

El paralelimso nos permitira ejecutar multiples instricciones de manera casi simultanea 

### Computador basico simplificado
+ Para poder generar el paralelismo modificaremos el computador basico hacia una version simplificada de este.
	+ ALU se simplifica y solo recibe lo de los registros y el selectALU, y su resultado tambien se usa como direcion de la mem. d datos
	+ Se elimina el manejo de la mem. stack
	+ Se eliminan las sub-rutinas
	+ Se implementa una nueva unidad para saltos condicionales, la *Jump Unit* 
	+ Se elimina el reg. Status, ahora los saltos deben incluir el CMP para actualizar el flag y mandarlo a la Jump Unit
+ Con estas modificaciones ahora la arquitectura pasa a ser de tipo Register-Register // Load-Store.
+ Esto nos permite separar la ejecion de una instruccioon en etapas
### Etapas de ejecucion
+ **Instruction Fetch (IF)** : Obtencion de opcode y literal
+ **Instruction Decode (ID)** : Decodificacion del opcode en señales de control
+ **Executte (EX)** : Ejecucion de la instruccion en la ALU
+ **Memory (MEM)**: Lectura o escritura en memoria de datos
+ **Writeback (WB)**: Escritura en registros
### Pipeline
+ Un **pipeline** es un proceso secuencial con etapas independientes
+ Cada etapa tiene una duracion distinta siendo las de IF y MEM las mas lentas
+ La cantidad maxima de instrucciones simultaneas que se pueden ejecutar depende de la cantidad de etapas del pipeline
+ La velocidad de pipeline proviene de la velocidad del clock
+ Para implementar pipeline en el hardware necesitamos:
	+ Se añaden 4 registros correspondientes a cada etapa IF/ID, ID/EX, EX/MEM, MEM/WB.
	+ Estos se actualizan con cada flanco de subida del clock
	+ Ahora cada instruccion se demora 5 ciclos en terminar
### Hazards 
+ Un hazard es un problema relacionado a la dependencia entre instrucciones
+ **Hazards de datos** 
	+ Ocurren cuando existe dependencia de datos entre instrucciones
	+ Solucion: Agregar una nueva unidad, la Forwarding Unit
	+ ***Forwarding Unit*** : 
		+ nos permite aprovecharnos de que valor actualizado se calcula en la etapa EX y se queda almacenado en los registros intermedios
		+ Para solucionar el caso de querer escribir en memoria  el valor de un registro acrtualizado recientemente añadimos una segunda forwarding unit la cual transmite el resultado de la etapa MEM en vez de la etapa EX
	+ Si hacemos una lectura de memeoria y queremos usar ese resultado en la siguiente instruccion tenemos el problema de que se termina la lectura del dato al mismo tiempo que se realiza la ejeccion de la siguiente instruccion, este problema no es solucionable con forwarding
	+ ***Stalling por software*** :
		+ Consiste en obligar a que la instruccion espere por una iteracion
		+ "Insertar NOP"
	+ ***Stalling por hardware*** :
		+ Similar a su idem de software
		+ El hardware genera una burbuja en el pipeline para que la instruccion deje de avanzar por una iteracion
		+ Para hacer esta operacion por hardware necesitamos un nuevo componente la **Data Hazard Unit**
		+ if (ID/EX.LoadA == 1 and ID/EX.RegIn == Dout and ID.AluA == A) or (ID/EX.LoadB == 1 and ID/EX.RegIn == Dout and ID.AluB == B) → Inserción de burbuja
+ **Hazards de control**
	+ Ocurren con las instrucciones de salto
	+ **Stalling**: esperar hasta realizar o no el salto, pero estos no siempre son utiles ya que la cantidad de ciclos depende del pipeline
	+ En la practica se usan **Unidades de Predicción de salto**
	+ **Unidades de Prediccion de salto** : 
		+ el hardware decide si saltar de inmediato o no. Para el curso se asume que nunca hay salto
	+ **Flushing** :
		+ En caso de que nos equivoquemos con la prediccion borramos el avancce y retomamos desde el salto
+ **Hazards estructurales**
	+ Ocurren cuando dos etapas necesitan utilizar una misma unidad funcional
	+ Se puede solucionar usando *stalling* o agregando otra unidad de memoria, pero no son buenas soluciones
	+ La solucion correcta es usar una **cache split**
	+ **Cache Split**:
		+ Permiten 2 accesos de memoria concurrentes uno para datos y otro para instrucciones
### Computador basico con Pipeline
+ **ISA**:
	+ ya no existen operaciones con uso del output de memoria
