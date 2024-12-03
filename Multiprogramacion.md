## Multiprogramación

Simulación de procesamiento paralelo que lleva a cabo el computador. Se basa en la ejecución intercalada de los programas. Si bien no es 100% ejecución paralela, para el usuario se percibe como paralelismo real.

### Memoria Virtual:
- **Espacio de memoria direccionable** que se traduce al espacio direccionable de la memoria principal.
- **MMU (Memory Management Unit):** Encargada de traducir direcciones físicas a memoria principal.
- El **espacio de memoria virtual** puede ser mayor al de la memoria física, permitiendo al computador ocupar más espacio del que tiene físicamente.

### Traducción de Direcciones:
- La MMU tiene una tabla que funciona como un diccionario asociando direcciones virtuales y físicas.
- Estas asociaciones no son 1:1, ya que se usa el método de **Paginación**:
  - La memoria virtual se divide en bloques contiguos llamados **páginas**.
  - Cada página se mapea a un **marco** de la memoria física (comparten tamaño).
  - Los bits de la dirección virtual se separan en **número de página** y **offset** de la palabra buscada en la página.

### Tabla de Páginas:
- Se encuentra en la sección protegida de la memoria y es almacenada en el registro especial **PTBR** (*Page Table Base Register*).
- Cada entrada de la tabla (**PTE**, *Page Table Entry*) incluye:
  - Número de marco físico.
  - Bits de metadata asociados a la página.

### Bits de Metadata:
- Proporcionan información sobre la asociación entre páginas y marcos físicos.
- **Present Bit:** Indica si el contenido de la entrada está en un marco físico o en el *swap file*.

### Page Fault:
- Ocurre si el contenido de una página está en el *swap file*.
- **Solución:**
  - **Swap in:** Copiar el contenido desde el *swap file* a un marco disponible.
  - **Swap out:** Si no hay marcos disponibles, se libera uno antes de realizar el *swap in*.

### Páginas sin Marco Físico:
- Son páginas cuyos datos no están en la memoria principal y requieren ser traídos desde el almacenamiento secundario.

### Accesos a Memoria:
- Para obtener un dato en la memoria principal, necesitamos al menos 2 accesos:
  1. Uno para consultar la tabla de páginas.
  2. Otro para acceder al dato deseado.
- **Solución:** Usar una memoria caché especial, la **TLB (Translation Lookaside Buffer):**
  - Es una memoria pequeña que almacena traducciones recientes.
  - **Fully associative**, con niveles (L1, L2, L3).
  - Almacena lo mínimo necesario para la traducción y requiere un **bit de validez**.
- Problema de cambio de contexto:
  - Las entradas de la TLB pueden volverse inválidas en el nuevo proceso.
  - **Soluciones:**
    - **Flush:** Vaciar la TLB (establecer a 0 el *valid bit* de todas las entradas).
    - **ASID:** Usar un identificador para cada proceso.

- **Dirty Bit:** Indica si un marco físico ha sido modificado, optimizando el *swapping* al evitar escribir páginas no modificadas en el *swap file*.

### Tamaño de la Tabla de Páginas:
- El tamaño de la tabla depende de:
  - **Tamaño de la memoria virtual.**
  - **Tamaño de la página.**
  - **Número de bits por entrada en la tabla (PTE).**
- Fórmula:
  \[
  \text{Tamaño de la Tabla de Páginas} = \frac{\text{Tamaño de la Memoria Virtual}}{\text{Tamaño de Página}} \times \text{Tamaño de una PTE}
  \]
- **Ejemplo:**
  - Si la memoria virtual es de \( 4 \, \text{GB} \), el tamaño de página es \( 4 \, \text{KB} \) y cada PTE ocupa \( 4 \, \text{bytes} \):
    \[
    \text{Tamaño de la Tabla} = \frac{4 \, \text{GB}}{4 \, \text{KB}} \times 4 \, \text{bytes} = 4 \, \text{MB}
    \]
