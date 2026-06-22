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

<details>
<summary><b>VA0 5V</b></summary>

Esta placa usa los chips _**MX29F1610MC**_ de 2mb (o 2048kb), se debe programar con la BIOS deseada antes de colocarlo en la consola.

</details>


https://www.youtube.com/watch?v=g9755sn-8no VA0 ModzvilleUSA!
https://youtu.be/3uDJ-q2Sw2o?si=I_xdVHWrhJVvFiAo MOD SPI SD Dreamcast Mundo Yakara
