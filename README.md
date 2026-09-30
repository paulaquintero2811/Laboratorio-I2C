# Laboratorio I2C — Raspberry Pi Pico y pantalla OLED

## Descripción

En este laboratorio se implementó y analizó la comunicación mediante el protocolo **I2C** entre una **Raspberry Pi Pico** y una pantalla OLED **SSD1306 de 128 × 32 píxeles**.

La comunicación fue programada utilizando **MicroPython** y posteriormente analizada mediante un **analizador lógico y el software Logic 2**. Se estudiaron las señales **SDA y SCL**, la dirección del dispositivo, las condiciones de inicio y parada, los datos transmitidos y las respuestas de confirmación **ACK/NACK**.

También se realizaron diferentes pruebas de control de la pantalla OLED, incluyendo encendido, apagado, contraste, inversión de colores, limpieza, visualización de texto y una animación.

## Objetivos

### Objetivo general

Implementar y analizar la comunicación I2C entre un microcontrolador y una pantalla OLED SSD1306 mediante MicroPython y un analizador lógico, con el fin de comprobar el funcionamiento de sus comandos de control y estudiar las señales involucradas en la transmisión.

### Objetivos específicos

* Desarrollar un programa en MicroPython para controlar la pantalla OLED mediante un menú interactivo.
* Implementar las funciones de encendido, apagado, contraste, inversión de colores, limpieza, texto y animación.
* Analizar las señales SDA y SCL mediante Logic 2.
* Identificar la dirección del dispositivo, los datos transmitidos y las respuestas ACK.
* Modificar la frecuencia del bus I2C y comparar el valor configurado con el valor medido experimentalmente.

## Hardware utilizado

* Raspberry Pi Pico
* Pantalla OLED SSD1306 de 128 × 32 píxeles
* Analizador lógico
* Protoboard
* Computador
* Cables de conexión

## Software utilizado

* MicroPython
* Thonny
* Logic 2

## Protocolo I2C

I2C es un protocolo de comunicación serial síncrono que utiliza principalmente dos líneas:

* **SDA:** línea de datos.
* **SCL:** línea de reloj.

La línea SDA transporta la dirección, comandos y datos, mientras que SCL proporciona la señal de reloj que sincroniza la comunicación.

## Conexiones

La pantalla OLED se conectó a la Raspberry Pi Pico mediante cuatro conexiones:

| Pantalla OLED | Raspberry Pi Pico | Función            |
| ------------- | ----------------- | ------------------ |
| VCC           | 3V3(OUT)          | Alimentación       |
| GND           | GND               | Tierra común       |
| SDA           | GPIO 14 (GP14)    | Línea de datos I2C |
| SCL           | GPIO 15 (GP15)    | Línea de reloj I2C |

## Análisis de la trama I2C

Mediante Logic 2 se analizaron las señales SDA y SCL y se utilizó el decodificador de protocolo I2C para identificar:

* Condición **START**
* Dirección del dispositivo
* Bit de lectura/escritura
* Datos transmitidos
* Respuestas **ACK/NACK**
* Condición **STOP**

Esto permitió relacionar el código ejecutado en MicroPython con las señales físicas observadas en el bus I2C.

## Dirección de la pantalla OLED

Mediante un escaneo del bus I2C se identificó la dirección:

```text
0x3C
```

Esta dirección correspondió a la pantalla OLED, ya que fue la que respondió correctamente con un **ACK** durante el escaneo.

## Pruebas ACK/NACK

Se realizaron dos pruebas para comprobar el reconocimiento del dispositivo:

1. Se utilizó la dirección correcta de la pantalla OLED y se obtuvo una respuesta **ACK**.
2. Se modificó la dirección por una incorrecta para observar la respuesta **NACK**.

Las dos situaciones fueron capturadas mediante Logic 2 para analizar las diferencias en la comunicación.

## Pruebas de control de la pantalla

Se realizaron diferentes pruebas mediante el menú desarrollado en MicroPython.

### Encendido y apagado

Se analizaron los comandos utilizados para controlar el estado de la pantalla:

* `0xAE` → apagar pantalla.
* `0xAF` → encender pantalla.

### Contraste

Se utilizó el comando:

```text
0x81
```

para configurar el contraste de la pantalla.

### Inversión de colores

Se analizaron los comandos:

```text
0xA7 → activar inversión
0xA6 → visualización normal
```

Estos comandos permitieron comprobar mediante Logic 2 la relación entre las instrucciones enviadas por el microcontrolador y el comportamiento observado en la pantalla.

## Visualización de texto

Para la prueba de texto se utilizó la dirección:

```text
0x3C
```

seguida del byte de control:

```text
0x40
```

Este byte indica que la información transmitida corresponde a datos de pantalla. Los caracteres son convertidos en información gráfica y enviados al controlador SSD1306 mediante múltiples bytes.

## Animación

También se implementó una animación breve de aproximadamente **3 segundos**. Durante su ejecución se analizaron las señales SDA y SCL mediante Logic 2 para observar la transmisión de los datos necesarios para actualizar la pantalla.

## Cambio de frecuencia del bus I2C

Inicialmente se trabajó con una frecuencia de:

```text
100 kHz
```

Posteriormente se modificó la configuración a:

```text
50 kHz
```

Para comprobar experimentalmente el cambio se midió el período de la señal SCL mediante el analizador lógico.

### Resultados

| Parámetro              |     Valor |
| ---------------------- | --------: |
| Frecuencia configurada |    50 kHz |
| Período teórico        |     20 µs |
| Período medido         |     21 µs |
| Frecuencia medida      | 47,62 kHz |
| Diferencia             |    4,76 % |

El valor experimental se encontró cercano al valor configurado, mostrando que la modificación de la frecuencia del bus se reflejó en la señal física SCL.

## Resultados principales

Durante la práctica se comprobó:

* La comunicación entre la Raspberry Pi Pico y la pantalla OLED mediante I2C.
* La utilización de SDA y SCL para la transmisión.
* La dirección `0x3C` de la pantalla OLED.
* La identificación de respuestas ACK y NACK.
* La transmisión de comandos y datos mediante códigos hexadecimales.
* El funcionamiento de las opciones de encendido, apagado, contraste, inversión, limpieza, texto y animación.
* La relación entre la frecuencia configurada en el programa y la frecuencia observada físicamente en SCL.

## Conclusiones

La práctica permitió comprender de manera experimental el funcionamiento del protocolo I2C y su aplicación para controlar una pantalla OLED mediante una Raspberry Pi Pico.

El uso del analizador lógico permitió identificar las diferentes etapas de la comunicación, incluyendo las condiciones START y STOP, la dirección del dispositivo, los comandos, los datos y las respuestas ACK.

También se comprobó que la dirección `0x3C` corresponde a la pantalla OLED utilizada y que el cambio de frecuencia del bus de 100 kHz a 50 kHz se reflejó en la señal SCL, aunque con una pequeña diferencia respecto al valor teórico.

Finalmente, la práctica permitió relacionar directamente el programa desarrollado en MicroPython con las señales eléctricas observadas en el bus I2C.

## Autores

**Paula Quintero**
**Ediem Valero**

Universidad Militar Nueva Granada
Ingeniería de Telecomunicaciones
Comunicación Digital
Bogotá, Colombia — 2026
