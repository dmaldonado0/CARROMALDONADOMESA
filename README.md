![image alt](https://github.com/DavidM2708/DavidM2708-Circuitos_Mesa_Maldonado/blob/445aa7ad51c4a87ae4bed16605c140ddb8756f26/COMPENSAR.png)
# PROYECTO FINAL MALDONADO-MESA

## Integrantes:

Daniel Alejandro Maldonado Silva
[danielamaldonado@ucompensar.edu.co](mailto:danielamaldonado@ucompensar.edu.co)

David Esteban Mesa González
[destebanmesa@ucompensar.edu.co](mailto:destebanmesa@ucompensar.edu.co)

## Asignatura:

SISTEMAS DIGITALES

## Docente:

Diego Alejandro Barragan Vargas

## INGENIERIA EN SISTEMAS

BOGOTA D.C
2026

## Descripción del Proyecto

Este proyecto presenta el diseño e implementación de un vehículo autónomo inteligente capaz de identificar diferentes colores mediante el sensor TCS3200 y ejecutar acciones específicas de navegación según el color detectado. El sistema integra componentes electrónicos, programación embebida y herramientas de desarrollo de software para crear una solución interactiva y funcional.

La detección de colores se realiza mediante el análisis de las componentes RGB reflejadas por una superficie. A partir de estos datos, el microcontrolador Arduino procesa la información y determina la acción correspondiente, permitiendo que el vehículo avance o realice cambios de dirección de forma autónoma.

Como complemento al sistema físico, se desarrolló una interfaz gráfica utilizando Python y Streamlit, la cual permite monitorear en tiempo real el color detectado por el sensor y visualizar el estado del vehículo. Adicionalmente, se implementó un chatbot informativo que proporciona explicaciones sobre los componentes, el funcionamiento y la lógica empleada en el proyecto.

Este trabajo representa una integración práctica de conceptos relacionados con sistemas embebidos, automatización, procesamiento de señales, comunicación serial y desarrollo de interfaces gráficas, demostrando cómo el hardware y el software pueden combinarse para construir soluciones tecnológicas capaces de interactuar con su entorno.

## Objetivos
- Objetivo General

Diseñar e implementar un vehículo autónomo capaz de detectar colores y ejecutar acciones específicas de movimiento mediante la integración de sensores, actuadores, sistemas embebidos e interfaces gráficas de monitoreo.

- Objetivos Específicos
Implementar un sistema de reconocimiento de colores utilizando el sensor TCS3200 basado en el análisis de componentes RGB.
Procesar los datos capturados por el sensor mediante un Arduino UNO para la toma de decisiones en tiempo real.
Controlar el movimiento de motores DC utilizando el driver L298N para ejecutar desplazamientos y cambios de dirección.
Diseñar una lógica de navegación basada en la identificación de colores previamente calibrados.
Desarrollar una interfaz gráfica utilizando Streamlit para visualizar el estado del sistema y los colores detectados en tiempo real.
Implementar un chatbot interactivo que permita consultar información relacionada con el funcionamiento del proyecto.
Integrar hardware y software en una única solución capaz de operar de manera autónoma.
Aplicar conocimientos de electrónica, programación, automatización y desarrollo de aplicaciones en un entorno práctico de aprendizaje.
Evaluar el desempeño del sistema mediante pruebas de detección y seguimiento sobre diferentes superficies y condiciones de iluminación.

### Tecnologías Utilizadas:
Hardware:

El sistema físico está compuesto por los siguientes elementos electrónicos:

Arduino UNO como unidad principal de procesamiento.

![image alt](https://github.com/dmaldonado0/CARROMALDONADOMESA/blob/main/ARDU.png)

Sensor de color TCS3200 para la detección de colores mediante lectura RGB.

![image alt](https://github.com/dmaldonado0/CARROMALDONADOMESA/blob/main/SENS.jpeg)

Driver L298N para el control de velocidad y dirección de los motores.

![image alt](https://github.com/dmaldonado0/CARROMALDONADOMESA/blob/main/LCN.webp)

Dos motores DC encargados de la locomoción del vehículo.

![image alt](https://github.com/dmaldonado0/CARROMALDONADOMESA/blob/main/MOTOSSSSSSSS.jpg)

Chasis robótico móvil como estructura de soporte.

![image alt](https://github.com/dmaldonado0/CARROMALDONADOMESA/blob/main/KIT.webp)

LEDs indicadores para la visualización local de los colores detectados.

![image alt](https://github.com/dmaldonado0/CARROMALDONADOMESA/blob/main/LRD.webp)

Portabaterías para la alimentación del sistema.

![image alt](https://github.com/dmaldonado0/CARROMALDONADOMESA/blob/main/Portabater%C3%ADas%20para%204%20pilas%2018650.%20En%20paralelo.jpg)

Baterías AA o baterías recargables tipo 18650.

![image alt](https://github.com/dmaldonado0/CARROMALDONADOMESA/blob/main/18.jpeg)

Cableado y elementos de conexión.

Software:

El desarrollo del sistema se realizó utilizando las siguientes herramientas:

Arduino IDE para la programación y carga del firmware en el microcontrolador.
Python 3 para la comunicación serial y procesamiento de información.
Streamlit para la construcción de la interfaz gráfica interactiva.
PySerial para la comunicación entre Arduino y Python.
Git y GitHub para el control de versiones y documentación del proyecto.

## CODIGO ARDUINO:

```cpp

#define IN1 8
#define IN2 9
#define IN3 10
#define IN4 11

#define S0 4
#define S1 5
#define S2 6
#define S3 7
#define sensorOut 2

#define LED_ROJO A0
#define LED_VERDE A1
#define LED_AZUL A2
#define LED_BLANCO A3
#define LED_NEGRO A4

int redFrequency = 0;
int greenFrequency = 0;
int blueFrequency = 0;

int leerSensor(int s2, int s3) {

  long suma = 0;

  for (int i = 0; i < 5; i++) {
    digitalWrite(S2, s2);
    digitalWrite(S3, s3);
    suma += pulseIn(sensorOut, LOW, 100000);
    delay(5);
  }

  return suma / 5;
}

// ==========================
// SETUP
// ==========================
void setup() {

  pinMode(IN1, OUTPUT);
  pinMode(IN2, OUTPUT);
  pinMode(IN3, OUTPUT);
  pinMode(IN4, OUTPUT);

  pinMode(S0, OUTPUT);
  pinMode(S1, OUTPUT);
  pinMode(S2, OUTPUT);
  pinMode(S3, OUTPUT);

  pinMode(sensorOut, INPUT);

  pinMode(LED_ROJO, OUTPUT);
  pinMode(LED_VERDE, OUTPUT);
  pinMode(LED_AZUL, OUTPUT);
  pinMode(LED_BLANCO, OUTPUT);
  pinMode(LED_NEGRO, OUTPUT);

  digitalWrite(S0, HIGH);
  digitalWrite(S1, LOW);

  Serial.begin(9600);
}

void adelante() {
  digitalWrite(IN1, LOW);
  digitalWrite(IN2, HIGH);
  digitalWrite(IN3, LOW);
  digitalWrite(IN4, HIGH);
}

void derecha() {
  digitalWrite(IN1, HIGH);
  digitalWrite(IN2, LOW);
  digitalWrite(IN3, LOW);
  digitalWrite(IN4, HIGH);
}

void izquierda() {
  digitalWrite(IN1, LOW);
  digitalWrite(IN2, HIGH);
  digitalWrite(IN3, HIGH);
  digitalWrite(IN4, LOW);
}

void apagarLEDs() {
  digitalWrite(LED_ROJO, LOW);
  digitalWrite(LED_VERDE, LOW);
  digitalWrite(LED_AZUL, LOW);
  digitalWrite(LED_BLANCO, LOW);
  digitalWrite(LED_NEGRO, LOW);
}

void loop() {

  redFrequency = leerSensor(LOW, LOW);
  greenFrequency = leerSensor(HIGH, HIGH);
  blueFrequency = leerSensor(LOW, HIGH);

  Serial.print("R=");
  Serial.print(redFrequency);
  Serial.print(" G=");
  Serial.print(greenFrequency);
  Serial.print(" B=");
  Serial.println(blueFrequency);

  apagarLEDs();


  int scoreBlanco = abs(redFrequency - 70) + abs(greenFrequency - 80) + abs(blueFrequency - 60);
  int scoreAzul   = abs(redFrequency - 175) + abs(greenFrequency - 160) + abs(blueFrequency - 108);
  int scoreNegro  = abs(redFrequency - 255) + abs(greenFrequency - 280) + abs(blueFrequency - 240);

  int minScore = scoreBlanco;
  String color = "BLANCO";

  if (scoreAzul < minScore) {
    minScore = scoreAzul;
    color = "AZUL";
  }

  if (scoreNegro < minScore) {
    minScore = scoreNegro;
    color = "NEGRO";
  }

  // ROJO / VERDE (simple comparativo)
  if (redFrequency < greenFrequency && redFrequency < blueFrequency) {
    color = "ROJO";
  }

  if (greenFrequency < redFrequency && greenFrequency < blueFrequency) {
    color = "VERDE";
  }


  if (color == "BLANCO") {

    Serial.println("BLANCO");
    digitalWrite(LED_BLANCO, HIGH);
    adelante();

  } else if (color == "AZUL") {

    Serial.println("AZUL");
    digitalWrite(LED_AZUL, HIGH);
    izquierda();

  } else if (color == "NEGRO") {

    Serial.println("NEGRO");
    digitalWrite(LED_NEGRO, HIGH);
    adelante();

  } else if (color == "ROJO") {

    Serial.println("ROJO");
    digitalWrite(LED_ROJO, HIGH);
    derecha();

  } else if (color == "VERDE") {

    Serial.println("VERDE");
    digitalWrite(LED_VERDE, HIGH);
    derecha();
  }

  delay(20);
}
```

# Funcionamiento del Algoritmo de Detección y Navegación

El programa desarrollado para Arduino es el encargado de coordinar todas las operaciones del vehículo autónomo, incluyendo la lectura del sensor de color, la identificación de colores, el control de motores y la comunicación con la interfaz de monitoreo.

## Adquisición de Datos del Sensor TCS3200

El sensor TCS3200 trabaja convirtiendo la intensidad de luz reflejada por una superficie en señales de frecuencia. Para identificar correctamente un color, el sistema realiza lecturas independientes de los componentes:

* Rojo (Red)
* Verde (Green)
* Azul (Blue)

Con el fin de mejorar la estabilidad de las mediciones y reducir el ruido producido por cambios de iluminación o vibraciones del vehículo, se implementó una función de promediado que realiza cinco lecturas consecutivas para cada componente RGB y calcula su valor promedio.

Este proceso permite obtener datos más precisos y consistentes durante el desplazamiento del carro.

---

## Identificación Inteligente de Colores

A diferencia de los métodos tradicionales basados únicamente en rangos fijos de valores RGB, este proyecto utiliza una estrategia basada en distancia matemática.

Para cada color previamente calibrado (Blanco, Azul y Negro), se almacenan valores de referencia obtenidos experimentalmente.

Posteriormente, se calcula qué tan cercana está la lectura actual a cada color utilizando la suma de diferencias absolutas entre los valores RGB medidos y los valores de referencia.

El color cuya distancia sea menor es considerado como el color detectado.

Este método ofrece varias ventajas:

* Mayor tolerancia a variaciones de iluminación.
* Mejor comportamiento en movimiento.
* Menor necesidad de recalibraciones constantes.
* Mayor estabilidad en la detección de colores.

Para los colores Rojo y Verde se utiliza una comparación directa entre componentes RGB, identificando cuál componente presenta la frecuencia dominante en cada lectura.

---

## Sistema de Navegación del Vehículo

Una vez identificado el color, el microcontrolador ejecuta una acción específica mediante el driver L298N, encargado de controlar los motores del vehículo.

Las acciones programadas son:

| Color Detectado | Acción Ejecutada     |
| --------------- | -------------------- |
| ⚪ Blanco        | Avanzar              |
| ⚫ Negro         | Avanzar              |
| 🔴 Rojo         | Girar a la derecha   |
| 🟢 Verde        | Girar a la derecha   |
| 🔵 Azul         | Girar a la izquierda |

Estas decisiones permiten que el vehículo siga una ruta determinada utilizando colores como referencias visuales de navegación.

---

## 💡 Sistema de Indicadores LED

Para facilitar la supervisión del sistema durante las pruebas, se implementó un conjunto de LEDs indicadores.

Cuando un color es identificado:

* Se apagan todos los LEDs.
* Se enciende únicamente el LED correspondiente al color detectado.

Esto proporciona una retroalimentación visual inmediata sobre el estado del reconocimiento realizado por el sensor.

---

## Comunicación con la Interfaz de Monitoreo

El programa envía continuamente información a través del puerto serial utilizando comunicación UART a 9600 baudios.

Los datos transmitidos incluyen:

* Valores RGB obtenidos por el sensor.
* Color detectado por el algoritmo.
* Estado actual del vehículo.

Estos datos son posteriormente recibidos por una aplicación desarrollada en Python utilizando la librería PySerial y mostrados en una interfaz gráfica creada con Streamlit.

Gracias a esta integración es posible visualizar en tiempo real el comportamiento del sistema mientras el vehículo se encuentra en funcionamiento.

---

## Optimización Implementada

Durante el desarrollo se realizaron diversas mejoras para aumentar la precisión y estabilidad del sistema:

* Promediado de lecturas RGB para reducir ruido.
* Detección basada en distancia matemática en lugar de rangos rígidos.
* Reducción del tiempo de respuesta mediante ciclos de lectura optimizados.
* Indicadores LED para depuración visual.
* Comunicación serial en tiempo real con la interfaz de monitoreo.

Estas optimizaciones permitieron obtener una detección más rápida, precisa y confiable durante las pruebas realizadas sobre diferentes superficies y condiciones de iluminación.

## VIDEO FUNCIONAMIENTO:

https://unipanamericanaeduco-my.sharepoint.com/:f:/g/personal/danielamaldonado_ucompensar_edu_co/IgD0dz7WId3BQb2n8sBWqj1KAdguYiyJP1YMI7jyml-FJnM?e=yju6PV

## CODIGO PYTHON-STREAMLIT
```
import streamlit as st
import serial
import time


st.set_page_config(
    page_title="Carro Inteligente",
    page_icon="🚗",
    layout="centered"
)

st.title("🚗¡CARRO MALDONADO-MESA DETECTOR DE COLORES!")

st.subheader("🎨 UUULTIMO COLOR MANINNN")


if "arduino" not in st.session_state:

    try:

        st.session_state.arduino = serial.Serial(
            'COM3',   # CAMBIAR SI ES NECESARIO
            9600,
            timeout=1
        )

        time.sleep(2)

    except Exception as e:

        st.error(f"Error COM: {e}")
        st.stop()

arduino = st.session_state.arduino


if "ultimo_color" not in st.session_state:

    st.session_state.ultimo_color = "⚪ SIN DETECCIÓN"


try:

    while arduino.in_waiting > 0:

        dato = arduino.readline().decode(
            'latin-1'
        ).strip()

        # DEBUG OPCIONAL
        # st.write(dato)

        if "ROJO" in dato:

            st.session_state.ultimo_color = "🔴 ROJO"

        elif "VERDE" in dato:

            st.session_state.ultimo_color = "🟢 VERDE"

        elif "AZUL" in dato:

            st.session_state.ultimo_color = "🔵 AZUL"

        elif "NEGRO" in dato:

            st.session_state.ultimo_color = "⚫ NEGRO"

        elif "BLANCO" in dato:

            st.session_state.ultimo_color = "⚪ BLANCO"

except:
    pass

color = st.session_state.ultimo_color

if "ROJO" in color:

    st.error(color)

elif "VERDE" in color:

    st.success(color)

elif "AZUL" in color:

    st.info(color)

elif "NEGRO" in color:

    st.warning(color)

elif "BLANCO" in color:

    st.success(color)

else:

    st.write(color)


st.divider()

st.subheader("📋 Preguntas disponibles")

preguntas = [

    "¿Qué hace el sensor TCS3200?",
    "¿Qué hace Arduino en el proyecto?",
    "¿Para qué sirve el L298N?",
    "¿Cómo funciona el carro?",
    "¿Qué colores detecta?",
    "¿Cómo funciona Streamlit?",
    "¿Qué hace Python?",
    "¿Cómo detecta colores?",
    "¿Qué motores usa?",
    "¿Cómo se mueve el carro?",
    "¿Qué sucede con el color rojo?",
    "¿Qué sucede con el color azul?",
    "¿Qué sucede con el color verde?",
    "¿Qué sucede con el color negro?",
    "¿Qué sucede con el color blanco?",
    "¿Cómo funciona el sistema RGB?",
    "¿Qué es un sensor de color?",
    "¿Cómo se conectan los motores?",
    "¿Cómo funciona la interfaz?",
    "¿Qué componentes tiene el proyecto?",
    "¿Cómo funciona el driver L298N?",
    "¿Qué es un motor DC?",
    "¿Cómo se alimenta el carro?",
    "¿Qué lenguaje usa el proyecto?",
    "¿Qué hace el monitor serial?",
    "¿Cómo detecta el negro?",
    "¿Cómo detecta el blanco?",
    "¿Qué función cumplen los LEDs?",
    "¿Cómo se comunican Arduino y Python?",
    "¿Qué ventajas tiene Streamlit?"

]

pregunta = st.selectbox(
    "Selecciona una pregunta",
    preguntas
)



def preguntar_ia(mensaje):

    mensaje = mensaje.lower()

    # RESPUESTAS

    if "tcs3200" in mensaje:

        return """
        El sensor TCS3200 detecta colores
        utilizando filtros RGB y convierte
        la luz en frecuencias.
        """

    elif "arduino" in mensaje:

        return """
        Arduino controla los motores,
        procesa datos del sensor
        y toma decisiones de movimiento.
        """

    elif "l298n" in mensaje:

        return """
        El L298N es un driver de motores
        que controla dirección y movimiento.
        """

    elif "funciona el carro" in mensaje:

        return """
        El carro detecta colores
        y se mueve dependiendo
        del color detectado.
        """

    elif "colores detecta" in mensaje:

        return """
        Detecta:
        rojo, verde, azul,
        negro y blanco.
        """

    elif "streamlit" in mensaje:

        return """
        Streamlit crea la interfaz gráfica
        para visualizar datos
        en tiempo real.
        """

    elif "python" in mensaje:

        return """
        Python conecta Arduino
        con la interfaz desarrollada
        en Streamlit.
        """

    elif "detecta colores" in mensaje:

        return """
        El sensor analiza frecuencias RGB
        para identificar colores.
        """

    elif "motores" in mensaje:

        return """
        El proyecto utiliza motores DC
        controlados mediante L298N.
        """

    elif "mueve el carro" in mensaje:

        return """
        Dependiendo del color:
        avanza o gira.
        """

    elif "rojo" in mensaje:

        return """
        El color rojo hace
        que el carro gire
        a la derecha.
        """

    elif "azul" in mensaje:

        return """
        El color azul hace
        que el carro gire
        a la izquierda.
        """

    elif "verde" in mensaje:

        return """
        El color verde hace
        que el carro gire
        a la derecha.
        """

    elif "negro" in mensaje:

        return """
        El color negro hace
        que el carro avance.
        """

    elif "blanco" in mensaje:

        return """
        El color blanco hace
        que el carro avance.
        """

    elif "rgb" in mensaje:

        return """
        RGB significa:
        Red, Green y Blue.
        """

    elif "sensor de color" in mensaje:

        return """
        Un sensor de color detecta
        la luz reflejada
        por una superficie.
        """

    elif "interfaz" in mensaje:

        return """
        La interfaz muestra
        colores detectados
        en tiempo real.
        """

    elif "componentes" in mensaje:

        return """
        El proyecto usa:
        Arduino,
        TCS3200,
        L298N,
        motores DC,
        LEDs,
        Python y Streamlit.
        """

    elif "driver" in mensaje:

        return """
        El driver L298N controla
        el sentido de giro
        de los motores.
        """

    elif "motor dc" in mensaje:

        return """
        Un motor DC funciona
        mediante corriente continua
        para generar movimiento.
        """

    elif "alimenta" in mensaje:

        return """
        El carro puede alimentarse
        mediante baterías
        o pilas recargables.
        """

    elif "lenguaje" in mensaje:

        return """
        El proyecto utiliza:
        Arduino C++
        y Python.
        """

    elif "monitor serial" in mensaje:

        return """
        El monitor serial permite
        visualizar datos enviados
        por Arduino.
        """

    elif "detecta el negro" in mensaje:

        return """
        El negro refleja poca luz,
        generando frecuencias altas.
        """

    elif "detecta el blanco" in mensaje:

        return """
        El blanco refleja más luz,
        generando frecuencias bajas.
        """

    elif "leds" in mensaje:

        return """
        Los LEDs indican
        visualmente el color
        detectado.
        """

    elif "arduino y python" in mensaje:

        return """
        Arduino y Python se comunican
        mediante el puerto serial COM.
        """

    elif "ventajas" in mensaje:

        return """
        Streamlit permite crear
        interfaces rápidas,
        visuales e interactivas.
        """

    else:

        return """
        Proyecto basado en Arduino,
        Streamlit, Python y TCS3200.
        """

if st.button("Responder"):

    respuesta = preguntar_ia(
        pregunta
    )

    st.success(respuesta)


time.sleep(0.02)

st.rerun()

```
# Sistema de Monitoreo e Interfaz Gráfica

Como complemento al sistema embebido desarrollado en Arduino, se implementó una interfaz gráfica utilizando Python y Streamlit con el objetivo de visualizar en tiempo real el comportamiento del vehículo autónomo y proporcionar una herramienta interactiva para comprender el funcionamiento del proyecto.

Esta aplicación actúa como un puente entre el hardware y el usuario, permitiendo supervisar las decisiones tomadas por el sistema mientras el vehículo se encuentra en operación.

---

# Comunicación entre Arduino y Python

La comunicación entre el vehículo y la interfaz se realiza mediante comunicación serial a través del puerto COM.

Arduino transmite continuamente mensajes indicando:

* Valores capturados por el sensor de color.
* Color detectado en cada instante.
* Estado actual del sistema.

La aplicación desarrollada en Python utiliza la librería PySerial para recibir estos datos en tiempo real y procesarlos automáticamente.

Gracias a esta comunicación bidireccional es posible monitorear el comportamiento del vehículo sin necesidad de utilizar el monitor serial del Arduino IDE.

---

# Visualización de Colores en Tiempo Real

Una de las principales funciones de la interfaz consiste en mostrar el último color identificado por el sensor TCS3200.

Cada vez que Arduino detecta un color, envía una etiqueta de texto mediante el puerto serial. La aplicación interpreta dicha información y actualiza automáticamente la interfaz.

Los colores reconocidos por el sistema son:

* 🔴 Rojo
* 🟢 Verde
* 🔵 Azul
* ⚫ Negro
* ⚪ Blanco

Para facilitar la interpretación visual, cada color se presenta utilizando diferentes estilos y componentes gráficos proporcionados por Streamlit.

Esto permite conocer instantáneamente la decisión tomada por el sistema sin necesidad de revisar datos numéricos complejos.

---

# ⚡ Actualización Automática de Datos

La interfaz fue diseñada para operar en tiempo real.

Mediante ciclos de actualización automáticos, la aplicación verifica constantemente si existen nuevos datos enviados por Arduino.

Cuando se detecta un cambio de color:

1. Se recibe la información desde el puerto serial.
2. Se procesa el mensaje recibido.
3. Se actualiza el estado interno de la aplicación.
4. Se refleja inmediatamente en la interfaz gráfica.

Este mecanismo permite una supervisión continua del vehículo durante las pruebas y demostraciones.

---

# Chatbot Informativo Integrado

Además del monitoreo visual, la aplicación incorpora un chatbot educativo diseñado para responder preguntas relacionadas con el proyecto.

El chatbot permite consultar información sobre:

* Sensor TCS3200.
* Arduino UNO.
* Driver L298N.
* Motores DC.
* Comunicación serial.
* Streamlit.
* Python.
* Sistema RGB.
* Funcionamiento general del vehículo.
* Componentes electrónicos utilizados.

El usuario puede seleccionar una pregunta desde un menú desplegable y obtener una respuesta inmediata generada a partir de una base de conocimientos previamente definida.

---

# Función Educativa del Chatbot

El chatbot fue incorporado con fines académicos y de divulgación tecnológica.

Su propósito principal es:

* Explicar el funcionamiento de cada componente.
* Facilitar la comprensión del sistema.
* Servir como herramienta de apoyo durante exposiciones y demostraciones.
* Permitir que cualquier persona pueda comprender el proyecto sin necesidad de conocimientos avanzados de electrónica o programación.

De esta manera, el sistema no solo realiza tareas de detección y navegación, sino que también proporciona información técnica de manera interactiva.

---

# Beneficios de la Interfaz

La implementación de la interfaz desarrollada en Python y Streamlit aporta múltiples ventajas al proyecto:

* Monitoreo visual en tiempo real.
* Mayor facilidad para realizar pruebas y depuración.
* Interacción amigable con el usuario.
* Visualización clara de los colores detectados.
* Explicación interactiva mediante chatbot.
* Integración entre hardware y software.
* Presentación profesional del proyecto.

Gracias a esta integración, el vehículo autónomo no solo es capaz de detectar colores y ejecutar movimientos, sino también de comunicar su estado y funcionamiento de forma clara, visual e intuitiva.

## Arquitectura General del Sistema

![image alt](https://github.com/dmaldonado0/CARROMALDONADOMESA/blob/main/07f58145-184e-4859-a5a6-9dc218f21dfa.png)


## VIDEO FUNCIONAMIENTO:

https://unipanamericanaeduco-my.sharepoint.com/:f:/g/personal/danielamaldonado_ucompensar_edu_co/IgD0dz7WId3BQb2n8sBWqj1KAdguYiyJP1YMI7jyml-FJnM?e=yju6PV

# Conclusión

El desarrollo de este proyecto permitió diseñar e implementar exitosamente un carro autónomo capaz de detectar colores y ejecutar acciones específicas mediante la integración de un sensor TCS3200, un Arduino UNO y un driver L298N. Asimismo, la incorporación de una interfaz gráfica desarrollada en Python con Streamlit facilitó el monitoreo en tiempo real del sistema y mejoró la interacción con el usuario a través de un chatbot informativo. Los resultados obtenidos demuestran que es posible construir soluciones robóticas funcionales y educativas utilizando tecnologías de bajo costo, aplicando de manera práctica conocimientos de electrónica, programación y automatización, y sentando las bases para futuras mejoras e implementaciones más avanzadas. 
