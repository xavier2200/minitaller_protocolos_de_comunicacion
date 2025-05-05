# Tutorial básico protocolo I2C

Abrimos tinkercad

Instanciamos lo siguiente:

- Un arduino
- Un LCD con I2C

Debe quedar así:

![image](/fig/lcd_arduino.png)

Conectamos los pines ```SCL``` cable anaranjado, ```SDA``` cable verde y de alimentación. Debería quedar así:

![image](/fig/conexiones.png)

Es importante en la configuración del LCD configurar lo siguiente:
- Type -> PCF8574-based
- Address: 0x27

## Ahora vamos con el código

Primero instanciamos las bibliotecas necesarias:

```
#include <Wire.h>
#include <LiquidCrystal_I2C.h>
```
Se instancia la clase ```LiquidCrystal_I2C``` como lcd, definimos la direccion del esclavo y las dimensiones del lcd:

```
LiquidCrystal_I2C lcd(0x27, 16,2);
```

Por ejemplo, si nosotros colocamos en la configuración del lcd ```0x20``` como la dirección del esclavo, va a ser necesario colocar el mismo identificador cuando se instancia la clase.

Luego inicializamos la clase y encendemos la retroluminacion del LCD:

```
void setup() {
  // put your setup code here, to run once:
  lcd.init();
  lcd.backlight();

}
```

Recordemos que esa función ```setup()``` solo se ejecuta una vez cuando se enciende el arduino.

Luego en el ```loop()``` vamos a imprimir cada 15ms un ```Hola Mundo```.
```
void loop() {
  // put your main code here, to run repeatedly:
  lcd.setCursor(0,0);
  lcd.print("Hola Mundo");
  delay(15);
}
```

Como eso no tiene mucha gracia les queda como tarea instanciar otro lcd asignandole la direccion ```0x20``` y crear un contador de segundos. 

Uno de los lcd va a estar atrasado por 5 segundos.

Como se debería de ver:

![image](/fig/lcd_complex.png)