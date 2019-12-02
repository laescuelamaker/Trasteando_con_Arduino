# Wemos Wifi con pantalla OLED #

![WEMOS_OLED](https://www.prometec.net/wp-content/uploads/2017/11/HTB1QhVWQpXXXXXqXXXXq6xXFXXXf.jpg)

* **Características de la placa**
  * Conexión Micro USB.
  * Compatibilidad con el IDE de Arduino.
  * 8 pines que pueden funcionar como entrada/salida/PWM/I2C/SPI (excepto el D0)
  * 1 entrada analógica.
  * Pantalla OLED de 0.96″ y 128×64. 

## 1. Configuración de la placa ##

* ### 1.1 Instalar la placa NODEMCU en IDE de Arduino###

    Seguir los pasos del tutorial de [Prometec](https://www.prometec.net/esp8266-pluggin-arduino-ide/)

* ### 1.2 Descargar librería para pantalla OLED ###

  * [Enlace de Descarga](https://github.com/ThingPulse/esp8266-oled-ssd1306) librería pantalla oled ssd1306 

    Puedes encontrar ejemplos de como usar la pantalla en "Archivo->Ejemplos->ESP8266 and ESP32 Oled driver for SSD1306 display.

    **¡¡¡IMPORTANTE!!!**
    
    Para que funcionen cambia la línea
    ```csharp
    SSD1306  display(0x3c, D3, D5);    
    ```
    por 
    ```csharp
    SSD1306  display(0x3c, 5, 4);
    ```

* ## 2. Let´s go ##

Vamos a empezar por el principio, qué es lo mínimo que necesitamos para hacer funcionar nuestra pantalla y como mostrar nuestro primer texto

```csharp
#include <Wire.h>
#include "SSD1306.h"

SSD1306 display(0x3c, 5, 4); //GPIO 5 = D1, GPIO 4 = D2


void setup() 
{
  Serial.begin(115200); 
  delay(10);
  /* Inicializamos la pantalla */
  display.init();   //Inicia la pantalla
  display.flipScreenVertically();   // Da la vuelta verticalmente

  /* Mostramos la pantalla de bienvenida */
  display.clear();  //Limpia la pantalla


  /* Fuentes por defecto
    * ArialMT_Plain_8
    * ArialMT_Plain_16
    * ArialMT_Plain_24
  */
  display.setFont(ArialMT_Plain_16);
  display.drawString(0, 25, "La Escuela Maker");
  display.display();
  delay (3000); 

}

void loop() 
{

}
```

* ### 2.1 Cambiar tipo de letra ###

Podemos cambiar el tipo de letra con estos pasos:

* Entramos a la aplicacion web [AQUÍ](http://oleddisplay.squix.ch/#/home)  
* Seleccionamos el tipo de letra, estilo (negrita, cursiva...) y el tamaño de fuente. El resto lo dejamos igual
* Pulsamos en "Create"
* Copiamos todo el texto que nos genera
* Creamos un nuevo archivo que lo llamaremos "font.h" y copiaremos el contenido en él
* Incluimos el archivo con #include "font.h"

Un ejemplo

```csharp
#include <Wire.h>
#include "SSD1306.h"
#include "font.h"

SSD1306 display(0x3c, 5, 4); //GPIO 5 = D1, GPIO 4 = D2

void setup() 
{
  Serial.begin(115200); 
  delay(10);
  /* Inicializamos la pantalla */
  display.init();   //Inicia la pantalla
  display.flipScreenVertically();   // Da la vuelta verticalmente

  /* Mostramos la pantalla de bienvenida */
  display.clear();  //Limpia la pantalla


  /* Fuentes por defecto
    * ArialMT_Plain_8
    * ArialMT_Plain_16
    * ArialMT_Plain_24
  */
  display.setFont(Satisfy_Regular_16);  //Incluimos el tipo de fuente nueva
  display.drawString(5, 25, "La Escuela Maker");
  display.display();
  delay (3000); 

}

void loop() 
{

}

```

* ### 2.2 Incluir imágenes, Ooou yeah ###
  * Crear una imagen dentro de las dimensiones de 180x64px
  * Exportarla en Gimp como xbm
  * Abrirla con un editor de texto
  * Copiar el contenido
  * Crear un archivo en Arduino y pegar el contenido
  * incluir el archivo con #include "nombre_del_archivo"

```csharp
#include <Wire.h>
#include "SSD1306.h"
#include "imagen.h" // Incluimos archivo imagen.h

SSD1306 display(0x3c, 5, 4); //GPIO 5 = D1, GPIO 4 = D2

void setup() 
{
  Serial.begin(115200); 
  delay(10);
  /* Inicializamos la pantalla */
  display.init();   //Inicia la pantalla
  display.flipScreenVertically();   // Da la vuelta verticalmente

  /* Mostramos la pantalla de bienvenida */
  display.clear();  //Limpia la pantalla
  display.setContrast(255);
  display.drawXbm(37, 5, t3_logo_width, t3_logo_height, t3_logo_bits);
  display.display();
  delay(3000);
  display.clear();
  display.setFont(Creepster_Caps_Regular_24);
  display.drawString(15,24, "Ooou Yeah");
  display.display();


}

void loop() 
{

}
```


