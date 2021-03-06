En este tutorial vamos a explicar cómo **montar un servidor web para POython con Flask** en nuestra Raspberry Pi.

<div class="toc">

- [Servidor Web con Flask](#servidor-web-con-flask)
  - [Instalar Flask](#instalar-flask)
  - [Hola Mundo](#hola-mundo)
- [Encendido y apagado de un LED](#encendido-y-apagado-de-un-led)
- [Resumen](#resumen)
- [Ejercicios propuestos](#ejercicios-propuestos)

</div>

# Servidor Web con Flask

Flask es un microframework creado para facilitar el desarrollo de aplicaciones web en Python. Es utilizado normalmente para construir servicios web como APIs REST o aplicaciones de contenido estático.

![](img/flask.png)

## Instalar Flask

> Antes de instalar cualquier software es conveniente actualizar la Raspberry Pi como se explica en el tutorial [Raspberry Pi - Raspbian - Update](raspberry_pi-raspbian-update)

Una vez actualizada instalamos el servidor de Flask para Python 3.

```sh
pi@raspberrypi:~ $ sudo apt install python3-flask
```

## Hola Mundo

El primer ejemplo que vamos a crear es el típico "Hola Mundo". En este caso, vamos a crear un servicio que al acceder a una determinada URL se muestre por la pantalla dicho mensaje.

En primer lugar se importa la librería de Flask y se asigna a la variable `app` encargada entre otras cosas de ejecutar el servicio, como se observa en la condición del final.

Como hemos dicho, Flask se utiliza para servicios, en este caso, creamos el servicio sobre la URL principal `@app.route('/')` seguido de la función que ejecutará el servicio. En este caso lo único que hacemos será devolver el código HTML con el mensaje "Hola Mundo".

```python
from flask import Flask
app = Flask(__name__)

@app.route('/')
def home():
   return '<html><body>¡Hola Mundo!</body></html>'

if __name__ == '__main__':
   app.run(host='0.0.0.0',debug=True)
```

Por último jecuta el código haciendo clic en el botón de ejecutar "Run" y accede mediante el navegador a la dirección `localhost:5000`. También podrás acceder desde introduciendo la IP de tu Raspberry Pi desde otro dispositivo situado en la misma red local.

```
URL: localhost:5000
```

# Encendido y apagado de un LED

Vamos a realizar el encendido y apagado de un LED conectado al `Pin 11 - GPIO 17` de nuestra Raspberry Pi.

![](img/led-fritzing.png)

En la programación, añadimos la librería para controlar los pines GPIO así como el modo de pin.

A continuación se crean dos entradas de URL o endpoints para encender y apagar dicho LED. Además mostramos un mensaje por la pantalla de la web.

```python
from flask import Flask
app = Flask(__name__)

import RPi.GPIO as GPIO
GPIO.setwarnings(False)
GPIO.setmode(GPIO.BCM)
GPIO.setup(17, GPIO.OUT)
GPIO.output(17, GPIO.LOW)

@app.route('/on')
def on():
   GPIO.output(17, GPIO.HIGH)
   return '<html><body>Led Encendido</body></html>'

@app.route('/off')
def off():
   GPIO.output(17, GPIO.LOW)
   return '<html><body>Led Apagado</body></html>'

if __name__ == '__main__':
   app.run(host='0.0.0.0',debug=True)
```

Por último jecuta el código haciendo clic en el botón de ejecutar "Run" y accede mediante el navegador a ambas direcciones para encender y apagar el LED.

```
URL: localhost:5000/on
URL: localhost:5000/off
```

---

# Resumen

Con este sencillo ejemplo hemos visto la forma de controlar los pines GPIO a través de un sencillo servidor web en Flask con Python.

---

# Ejercicios propuestos

1.- Enciende varios LEDs utilizando diferentes endpoints en el servidor web.
