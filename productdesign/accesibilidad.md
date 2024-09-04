
![](https://lh7-us.googleusercontent.com/rwSNx36JoKUI5NL93Bwel7LMZVToC_n15grDNNi7GQis_XWEqqReCQinslOE5YnRcRdCPa1yLRMm8gb61btXbtXH9m-gs8XmVC4EC0JmKJRSMqtNwo5XShCB9S4JBJkpvCd70c3yM_ah2yNR7guqw5tw5A=s2048)

Hacer accesible tu producto durante su desarrollo es:

* más rápido
* más barato

[The Web Content Accessibility Guidelines (WCAG) 2.1](https://www.w3.org/TR/WCAG21/)

- **Perceivable**
- **Operable**
- **Understandable**
- **Robust**
### Screen readers

- **Android**
	- TalkBack
- **iPhone**
	- VoiceOver
- **Mobile browser**
	- VoiceOver
	- TalkBack
- **Desktop browser**: 
	- VoiceOver
	- JAWs
	- NVDA**‍**
- **Desktop**
	- VoiceOver
	- JAWs
	- NVDA

Hay diferentes niveles de accesibilidad

* Basic: A
* Strong: AA
* Highest: AAA

Cada nivel de accesibilidad tiene sus requisitos

Cada sistema operativo y hardware tiene sus propios requisitos, como por ejemplo [[android_kotlin]] o iOS

### Level A

* El contenido que no sea de texto
* Los formularios deben tener etiquetas e instrucciones
* La información debe ser accesible, alcanzable por tecnologías de asistencia como lectores de pantalla
* La información no debe ser transmitida únicamente por la forma, tamaño o color

### Checklist de accesibilidad

- [ ] Todas las interacciones activas deben hacer algo
- [ ] Test de lector de pantallas
- [ ] Contrast ratios: al menos 4.5:1 entre controles/texto y background
- [ ] Las imágenes también deben tener suficiente contraste
- [ ] Todos los elementos tappables deben ser al menos de 48x48 píxeles
- [ ] Mientras se escribe, nada debe cambiar en el contexto de la UI de forma automática. Los widgets deben evitar cmabiar el contexto de user sin confirmación por parte del user.
- [ ] Las acciones importantes deben poder ser corregidas o deshechas
- [ ] Si los fields de un form muestran errores, se debe sugerir una corrección si es posible
- [ ] Controles a prueba de daltonismo y escala de grises
- [ ] La UI debe ser regible y usable ante re-escalado de tamaños de fuentes y pantalla

Checklist de vision impairments

- [ ] Test con Android Simulate Color Space (Android) o Color Filter (iOS)
- [ ] Comprueba el contraste, debería ser 4.5 : 1
- [ ] Ofrece dark mode, pero no impongas
- [ ] Fuentes grandes
- [ ] Comprobar si es acorde al diseño UX

Gestos o gestures

Evalúa las tap áreas

En Material Design deben ser 48x
En Cupertino deben ser 44x

Evalúa los Gesture Widgets y ofrece customizaciones si fuera posible

![[mobilehands.png]]

Cognitives

- [ ] Permite la deshabilitación de animaciones
- [ ] Comprueba si el contenido resulta abrumador a la vista o al escuchar
- [ ] Ofrece tiempos para ciertas acciones
- [ ] Permite la customización si fuera posible

Es todo cuestión de concienciación y comunicación, tanto entre personas, código y producto final.

### Carga cognitiva o cognitive load

Reducir o gestionar la carga cognitiva

* Presta atención al diseño de la interfaz eliminando elementos gráficos y animaciones innecesarias. Asegúrate de que el texto sea legible y utiliza colores de alto contraste. Ten en cuenta el tamaño de los elementos importantes, como botones y texto, y proporciona el espacio en blanco adecuado en la interfaz. 
* Diseña una interfaz coherente para que los usuarios no tengan que gastar energía constantemente tratando de entender cómo funciona tu aplicación. Además, utiliza patrones de diseño familiares que los usuarios no tengan que aprender de nuevo. 
* Incluye solo la información esencial que no distraiga a los usuarios ni desvíe su atención del contenido o la tarea principal. 
* Recuerda la calidad del contenido del sitio web: debe ser simple, lo más breve posible y comprensible para todos.
* Simplifica tus formularios agregando solo los campos que sean necesarios para el proceso. Para formularios más largos, agrupa los campos relacionados en secciones y divide el formulario en pasos apropiados. 
* Proporciona retroalimentación para que los usuarios no tengan que preguntarse si su acción fue exitosa. Manténgalos informados sobre lo que está sucediendo y por qué. 
* Asegúrate de que la jerarquía de la información sea clara para que los usuarios sepan inmediatamente por dónde empezar, qué es lo más importante y qué deben abordar a continuación. 
* Visualiza los datos de una manera fácil y familiar. Utilice cuadros, íconos y gráficos que ayuden a los usuarios a comprender y absorber la información. Sin embargo, recuerde que los íconos, como los de navegación, se pueden entender mejor con etiquetas apropiadas. 
* Reduce la cantidad de acciones repetitivas que un usuario debe realizar. Si podemos recordar algunos datos o establecer una opción seleccionada con mayor frecuencia, hagámoslo. 
* Limita la cantidad de elementos que un usuario necesita recordar a la vez. Según la ley de Miller, esto suele ser entre 5 y 9 elementos, pero también se dice que hoy en día, podría ser tan solo de 4 a 7. 
* Aplica la ley de Hick. Reduzca la cantidad de opciones disponibles para simplificar la toma de decisiones. 
* Guía a los usuarios a través de nuevas funciones y aplicaciones si son nuevas para ellos. Utilice tutoriales e instrucciones claras.