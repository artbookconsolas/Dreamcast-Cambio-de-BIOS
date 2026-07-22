# Dreamcast - Cambio de BIOS ~~actualmente solo en placas VA0~~.
Documentación de cómo cambiar el chip BIOS en consolas Sega Dreamcast, NO se explicara nada sobre un dual bios.

---

## Prefacio
La necesidad de cambiar la BIOS puede ser porque la original esté corrupta o, lo que más veremos aquí, porque el lector de la consola está muerto. 

Considerando que los lectores alternativos cuestan la mitad de una consola vendida en calidad de piezas, y que por unos $20 USD aprox. se vende el *pickup* original (el láser, la pieza móvil que lee el disco quizas con que uso), urge otra forma de cargar juegos. Esto toma en cuenta la modernidad (memorias flash por todos lados) y que los CD-ROM dejaron de fabricarse comercialmente hace bastante tiempo.

---
##  Parte Técnica

Existen 3 revisiones generales de las placas madres de esta consola: las **VA0**, **VA1** y **VA2** (junto con una mini revisión VA2.1). La primera revisión (VA0) por lo general ocupa un chip BIOS de **5V**, mientras que las otras dos utilizan **3.3V**.

Al realizar el cambio de la BIOS de fábrica por un chip comercial, se deben realizar modificaciones específicas en las patas del integrado:

### 1. Configuración de Estado (WP / Reset)
Dependiendo del chip comercial que se utilice, se debe unir la línea **VCC** (Pin 23 de la BIOS) al pin 44 de la BIOS de 5V **WP** (Write Protect) o al pin 1 de la BIOS de 3.3V **Reset**. Esto se hace para evitar que el chip entre en modo de protección contra escritura o se reinicie inesperadamente durante la ejecución de la consola.

### 2. Habilitación de Escritura (Línea WE a Interfaz G1)
Para poder programar o flashear el chip instalado directamente desde la propia consola (usando herramientas como DreamShell), es necesario soldar un cable desde el pin 1 de la BIOS de 5V o el pin 44 de la BIOS de 3.3V **WE** (Write Enable) del nuevo chip hacia el **pin B14** de la **Interfaz G1** (el conector gigante donde va acoplado el lector de discos). 

Este cable es el que permite que el software tome el control del chip y autorice la inyección de nuevos datos.

> [!NOTE]
> Si quieres bloquear o no te interesa programar la BIOS desde la consola, une el pin **WE** a **VCC**.

### 3. Opciones de BIOS Custom (¿Qué archivo flashear?)

Una vez que el hardware está listo, necesitarás un archivo `.bin` para programar el chip. Dependiendo de tu objetivo (usar GDEMU, disco duro, o solo jugar copias), la *scene* ha desarrollado distintas opciones:

*   **Japanese Cake (JC):** Es similar a la BIOS stock pero con esteroides. Hace que la consola sea *Region Free*, fuerza la compatibilidad con cables VGA en juegos que no lo soportan, permite saltar la animación de inicio, y es **la BIOS recomendada si usas GDEMU**, ya que optimiza los tiempos de carga y compatibilidad.
*   **[DC-SWAT (DreamShell Bootloader)](https://github.com/DC-SWAT/DreamShell):** Esta es la BIOS obligatoria si vas a instalar un mod de Disco Duro (IDE/SATA) o el mod **SPI-SD**. En lugar de ir al menú original de la consola, esta BIOS busca inmediatamente la ruta del dispositivo de almacenamiento masivo para cargar el sistema operativo DreamShell.
*   **BIOS Originales Modificadas (DevKit / Katana 3D):** Son BIOS que mantienen el funcionamiento clásico de la consola (ideal para usar el lector de discos original) pero están parcheadas para ser *Region Free*, leer backups (MIL-CD) y cambiar la mítica animación de la espiral por la del kit de desarrollo (espacio negro) o la animación Katana en 3D.

> [!NOTE]
> Por temas de **Copyright** no puedo proporcionar enlaces de descarga de BIOS. Pero confío en que sabrás dónde buscar estos archivos.

> [!TIP]
> Si tu lector funciona, una BIOS DevKit es ideal por estética. Si tu lector murió y usarás el mod de la tarjeta SD, ve directo por la de DC-SWAT.

<details>
<summary><b>Sobre la BIOS DC-SWAT (DreamShell Bootloader)</b></summary>

Sí puedo ayudarte a buscar estos archivos, y a explicar qué significa cada nombre.

Si vas al repo de **DreamShell**, en la sección de descargas, existe un archivo. Bájalo, descomprímelo y ve a la ruta `DS -> firmware -> bios -> ds`. Esos archivos son los relevantes.
* Los con nombre `devkit` tienen la animación de una consola de desarrollo, y `retail` la de siempre (es solo estética).
* Los con nombre `nogdrom` no esperan la inicialización del lector; ideal si no tienes un lector operativo.
* Los con nombre `32mb` son para consolas a las que se ha aumentado la RAM de 16MB a 32MB. Solo úsalo en ese caso.

</details>

---
# Procedimiento

Primero, se debe mirar el voltaje del pin 23 **VCC**, para saber que chip se necesita instalar. Esto se recomienda hacer con la consola encendida y un multimetro, si tienes más experiencia puedes ver que conector de la fuente llega al pin 23 **VCC**

## VA0

![Placa madre VA0 fuera de la consola sin modificar](Fotos/VA0/VA0.jpg)

Esta placa tiene por lo general una BIOS de 5V, por lo que usa los chips _**MX29F1610MC**_ de 2 MB (2048 KB). Se debe programar con la BIOS deseada antes de colocar el chip en la consola.

> [!WARNING]
> **Peligro con el uso de chips de 3.3V (MX29LV160TMC) en placas VA0:**
> En muchos tutoriales generales se recomienda la memoria **MX29LV160TMC**, pero este chip opera a **3.3V** (diseñado para placas VA1/VA2 o que usen 3.3V). **No se recomienda instalarlo directamente en una placa VA0 de 5V** ya que la lógica sigue siendo de 5V. Aunque en algunos casos la consola pueda encender, alimentar una memoria de 3.3V con 5V sin un circuito de adaptación provocará fallos de lectura, congelamientos aleatorios o la degradación permanente de la memoria a corto/mediano plazo.

![Acercamiento al chip BIOS montado en la placa](Fotos/VA0/VA0-BIOS.jpg)

1. Se retira el chip original.
2. Se limpian los pads (hay que dejarlos lo más planos posible, sin estaño sobrante).
3. Respetando el **PIN 1** original, se suelda el reemplazo.
4. Para finalizar, se debe unir el **PIN 23** con el **PIN 44** (con un cable directo) y, opcionalmente, el **PIN 1** del chip al **PIN B14** del conector GD-ROM.

![Chip de reemplazo ya montado en la consola](Fotos/VA0/BIOS-5V-DS-VA0.jpg)

> [!NOTE]
> El puente del PIN 1 al PIN B14 del GD-ROM es para programar el chip desde la misma consola mediante software. Si prefieres no usar esa función, puedes omitir este cable.

> [!TIP]
> Existe un videotutorial (en inglés) de [ModzvilleUSA](https://youtu.be/g9755sn-8no?si=kTDuldTNbNEdo-22) sobre la instalación en la placa VA0.

## VA1
POR HACER

## VA2/2.1
POR HACER

---

## 🎁 Anexo: Mod SCI-SPI (Lector MicroSD Interno)

Si instalaste una BIOS con **DreamShell** o la versión **No-GD**, el complemento ideal para cargar tus respaldos y aplicaciones es la instalación de un adaptador SD usando la interfaz serial (SCI-SPI).

> [!NOTE]
> Aunque la velocidad de transferencia del puerto serial es limitada en comparación con un GD-EMU o un mod IDE, es la solución más económica y funcional para ejecutar juegos, homebrew y emuladores.

### Diagramas y Referencias
Para la instalación del conector o adaptador de tarjeta SD, se recomienda consultar los magníficos diagramas explicativos y tutoriales creados por **Yakara Colombia**, los cuales detallan los puntos exactos de soldadura en la placa madre:

* 📺 **Tutorial y esquemas paso a paso:** [Video de Yakara Colombia - Mod SCI-SPI](https://youtu.be/3uDJ-q2Sw2o?si=O8tltq6eckUouUv3)
* 📌 **Puntos clave de conexión:**
  * **Líneas SPI:** SCK, MISO, MOSI y CS conectadas a las líneas del puerto serial.
  * **Alimentación:** **3.3V** y **GND** tomados directamente de los rieles de alimentación de la consola.

> [!TIP]
> Asegúrate de utilizar cables cortos (de preferencia de menos de 10 cm) y bien blindados para evitar interferencias en la lectura de la tarjeta MicroSD.
