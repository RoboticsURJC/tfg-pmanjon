# 🤖 TFG: Sistema Robótico Asistencial Low-Cost para Detección de Caídas

**Trabajo de Fin de Grado — Ingeniería Robótica Software**

## 📋 Descripción del Proyecto
La clave de este TFG es demostrar que el verdadero valor de la robótica asistencial moderna no está en construir hardware de miles de euros, sino en el software y la integración inteligente de componentes baratos. 

Este repositorio alberga la investigación, la arquitectura de software y las pruebas de simulación orientadas a abaratar drásticamente el coste de los sistemas de ayuda para personas mayores y dependientes como el **Detector de Caídas**.

---

## 📚 Estado del Arte y Marco Teórico

### 1. Evolución y Vanguardia en Detección de Caídas
* **[Algoritmos clásicos de detección (2015 - ScienceDirect)](https://www.sciencedirect.com/science/article/pii/S1877050915023777)**
  * **Concepto:** Establece las bases matemáticas usando sensores inerciales (IMU) y algoritmos basados en umbrales (*thresholds*).
  * **Utilidad para mi TFG:** Me sirve como "estado del arte" e hilo conductor. Justifica por qué los sistemas antiguos daban falsos positivos al sentarse de golpe y por qué mi proyecto necesita dar el salto a arquitecturas de software más avanzadas (IA o fusión de sensores) para ser realmente útil.

* **[Robot de cuidado con Path-Planning y Detección (2024 - MDPI)](https://www.mdpi.com/2076-3417/14/6/2374)**
  * **Concepto:** Un robot móvil que patrulla la casa integrando navegación autónoma (SLAM) y algoritmos de detección visual (como YOLO) en tiempo real.
  * **Utilidad para mi TFG:** Es una referencia directa de arquitectura de software robótico. El procesamiento concurrente es viable: puedo separar en distintos nodos (por ejemplo, bajo ROS 2) el control de los motores, el mapeo de la habitación y el procesamiento de la cámara.

* **[EM-Fall: Embodied mmWave Sensing (2026 - arXiv)](https://arxiv.org/abs/2606.11109)**
  * **Concepto:** La tecnología más puntera. Integra un sensor de radar milimétrico (*mmWave*) sobre una base robótica móvil para detectar caídas incluso de noche o detrás de muebles, sin grabar imágenes.
  * **Utilidad para mi TFG:** Me da el "factor innovador" definitivo. Combina la movilidad de la robótica con la protección total de la privacidad. Me permite defender un enfoque ético: usar sensores asequibles (radares de bajo coste) que respetan la intimidad del mayor en general, sobre todo en zonas críticas como el baño.

### 2. Optimización de Software y Hardware Asequible
* **[Procesamiento Edge y Transformers para Wearables (2024 - World Scientific)](https://www.worldscientific.com/doi/full/10.1142/S0129065724500266)**
  * **Concepto:** Optimización de modelos de IA complejos para que corran localmente dentro de dispositivos de bajo consumo que la persona lleva puestos.
  * **Utilidad para mi TFG:** Fundamenta mi viabilidad económica y técnica. Justifica que no necesito depender de servidores en la nube caros y que introducen latencia. Muestra cómo programar el algoritmo directamente en el "borde" (*Edge Computing*), ideal si uso microcontroladores económicos como un ESP32 o Raspberry Pi.

* **[Evaluación de Modelos CNN - Redes Neuronales Convolucionales (2020 - IEEE)](https://ieeexplore.ieee.org/abstract/document/9144836)**
  * **Concepto:** Evaluación de arquitecturas de *Deep Learning* para interpretar imágenes y extraer esqueletos virtuales (*pose estimation*) que clasifican caídas.
  * **Utilidad para mi TFG:** Me ahorra tiempo de investigación. Si le pongo una cámara al sistema, este artículo me dice exactamente qué tipo de red neuronal es la más ligera y precisa para entrenar mi propio modelo o adaptar uno de código abierto.

### 3. Interacción, Control Móvil y Robótica de Proximidad
* **[Algoritmo basado en visión para robots móviles (2021 - IEEE)](https://ieeexplore.ieee.org/abstract/document/9450950)**
  * **Concepto:** Algoritmos de visión computacional diseñados específicamente para ejecutarse mientras la cámara está en movimiento.
  * **Utilidad para mi TFG:** Resuelve un problema de control crítico. Si mi robot detecta caídas mientras avanza, la vibración de las ruedas falsea los datos de la cámara. Este *paper* me da la base algorítmica para estabilizar la imagen o compensar la odometría del robot por software.

* **[Sinergia IoT: Dispositivo portátil y robot acompañante (2021 - IEEE)](https://ieeexplore.ieee.org/abstract/document/9561323)**
  * **Concepto:** Sinergia IoT. La comunicación asíncrona entre un *wearable* y un robot base.
  * **Utilidad para mi TFG:** Me permite plantear una arquitectura de "disparador y actuador". El profesor verá que domino la comunicación entre sistemas: el reloj o pulsera barato actúa como disparador de alerta y manda una señal por red local para que el robot móvil (actuador) vaya automáticamente a la habitación del usuario.

* **[ADAM-UC3M: Compañero robótico social (2024 - Frontiers)](https://www.frontiersin.org/journals/neurorobotics/articles/10.3389/fnbot.2024.1337608/full)**
  * **Concepto:** Diseño de un robot orientado a la interacción humana, enfocado en asistencia cognitiva y proximidad respetuosa.
  * **Utilidad para mi TFG:** Un excelente referente nacional sobre "Interacción Humano-Robot" (HRI). Me sirve para justificar el diseño de alto nivel: cómo programar las tolerancias de navegación para que el robot no invada el espacio personal del anciano, evitando que le asuste o le haga tropezar.

### 4. Otras ideas interesantes que vi y me sirvieron de ayuda
* **[Robot Bastón inteligente (2015 - IEEE)](https://ieeexplore.ieee.org/abstract/document/7258377)**
  * **Concepto:** Transformación de una herramienta tradicional de apoyo físico en un sistema sensorizado e inteligente.
  * **Utilidad para mi TFG:** Valida la idea de la "tecnología invisible". Si en el proyecto decido rediseñar con materiales asequibles y sensores baratos objetos que los mayores ya usan a diario (andadores, bastones), este artículo demuestra que es una línea de investigación académicamente sólida.

---

## 🛠️ Metodología: Estrategia de Reducción de Costes

A continuación se exponen los pasos y principios técnicos aplicados para replicar y optimizar las soluciones manteniendo el presupuesto mínimo:

### 1. Sustituir licencias caras por ROS 2 y código propio
* **La inspiración:** El artículo de 2024 sobre el robot con *Path-Planning* y detección de caídas, y el proyecto ADAM.
* **Cómo abaratar costes:** Los robots asistenciales comerciales cuestan una fortuna porque te cobran su software propietario de navegación. Yo puedo esquivar esto estructurando toda la arquitectura del sistema mediante nodos de ROS 2. Podría programar los publicadores y suscriptores en C++ o Python para controlar la base móvil, gestionar la odometría y recibir los datos de la cámara. Al usar herramientas *open-source*, el coste del "cerebro" del robot se reduce a cero.

### 2. Procesamiento Local (*Edge*) vs. Servidores en la Nube
* **La inspiración:** El artículo de 2024 sobre *Edge Computing* para *wearables*.
* **Cómo abaratar costes:** Muchos proyectos de monitorización dependen de enviar datos a servidores en la nube (AWS, Google Cloud) que requieren suscripciones de pago y conexión constante a internet. Siguiendo el enfoque *Edge*, puedo programar la lógica de detección directamente en un microcontrolador de 5€ (como un ESP32) usando C. El dispositivo procesa los datos del acelerómetro localmente y solo usa el WiFi para mandar la alerta final, ahorrando costes de servidor y garantizando la privacidad del usuario.

### 3. Sensores "Tontos" + Visión Artificial Inteligente
* **La inspiración:** El modelo CNN de 2020 y los algoritmos basados en visión.
* **Cómo abaratar costes:** Un sistema de detección de caídas médico por infrarrojos o láser LIDAR puede costar cientos de euros. En su lugar, puedo comprar una webcam genérica (15€) y aplicar algoritmos de visión artificial. Entrenando una red neuronal ligera o usando librerías preentrenadas (como YOLO u OpenPose) ejecutadas en una Raspberry Pi, podría extraer o modelar el "esqueleto" de la persona a partir de un vídeo de baja calidad. El coste baja drásticamente porque la "inteligencia" es mía, mediante código, no comprando un hardware sofisticado.

### 4. Simulación antes de comprar tornillos
* **La inspiración:** La necesidad de probar algoritmos de navegación del robot móvil y la interacción del "Robot Acompañante".
* **Cómo abaratar costes:** Antes de gastar un solo céntimo en motores, perfiles de aluminio o imprimir piezas en 3D, modelaré todo el sistema posible en un entorno virtual. Usando simuladores dinámicos me aseguraré de que el código de control y evasión de obstáculos funciona perfectamente. Si el robot choca o falla al detectar una caída en la simulación, lo corrijo en el código sin haber roto ningún componente físico, generando menos pérdidas económicas.

---

## 📌 Conclusión General
En resumen, la literatura científica actual me ha servido para justificar sólidamente que voy a sustituir hardware carísimo por microcontroladores baratos, cámaras estándar y sensores de radar, uniendo todo ese "hardware barato" mediante una arquitectura de software robusta diseñada por mí.
