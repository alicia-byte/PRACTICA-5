# Informe Práctica 5
## Ejercicio Practico 1 ESCÁNER I2C
###  Descripción de la salida por el puerto serie
Cuando se ejecuta el código  se puede observar por el monitor serial una serie de mensajes con la siguiente estructura:
![salida puerto serie](https://i.imgur.com/v9tjwJA.jpeg)

Esto indica que se han detectado dos dispositivos I2C conectados al bus: el display LCD y el sensor AHT20. Asi como también aparece un mensaje de confirmación de que el sensor AHT ha sido encontrado correctamente.

###  Explicación del funcionamiento
Este programa utiliza la librería `Wire.h` para escanear todas las direcciones posibles del bus I2C. La ESP32 intenta comunicarse con cada dirección, y si recibe una respuesta (es decir, si no hay error), la considera válida y la muestra por el puerto serie.

Solo se necesitan dos cables para conectar todos los dispositivos I2C: uno para datos (SDA) y otro para reloj (SCL). 

## Ejercicio Practico 2

###  Código usado
El código que se ha utilizado muestra la temperatura y la humedad en una pantalla LCD, y también por el monitor serie.
```cpp
#include <Arduino.h>
#include "I2C_LCD.h"
#include <Adafruit_AHTX0.h>

Adafruit_AHTX0 aht;
I2C_LCD lcd(39);  // Dirección I2C del LCD

void setup() {
  Serial.begin(115200);
  lcd.config(39, 2, 1, 0, 4, 5, 6, 7, 3, POSITIVE);
  Wire.begin(21,20);
  lcd.begin(20, 4);

  if (!aht.begin()) {
    Serial.println("Sensor AHT no encontrado");
    while (1);
  }
  Serial.println("AHT10 o AHT20 encontrado");
}

void loop() {
  sensors_event_t humedad, temp;
  aht.getEvent(&humedad, &temp);

  lcd.setCursor(3,0);
  lcd.print("Temp:");
  lcd.setCursor(9,0);
  lcd.print(temp.temperature);

  lcd.setCursor(3,1);
  lcd.print("Hum:");
  lcd.setCursor(9,1);
  lcd.print(humedad.relative_humidity);

  Serial.print("Temperature: ");
  Serial.println(temp.temperature);
  Serial.print("Humidity: ");
  Serial.println(humedad.relative_humidity);

  delay(500);
}
```

###  Explicación fácil del código
El programa hace tres cosas:
  -   Inicializa el sensor AHT20 y el display LCD.
  -  Lee los datos de temperatura y humedad del sensor.
  -  Muestra esos datos en la pantalla LCD y también en el monitor serie del ordenador.
 
 También es necesario saber que el diplay se actualiza cada medio segundo.


###  Fotos del montaje
![display mostrando valores de humedad y temperatura](https://i.imgur.com/OoLDDSa.jpeg)


###  Salida por el monitor serie
En el monitor serie se ven los valores que el sensor mide:
```cpp
Temperature: 22.8 degrees C  
Humidity: 45.3% rH
```
Esto sirve para verificar que el sensor funciona bien y que los datos se están mostrando correctamente.
