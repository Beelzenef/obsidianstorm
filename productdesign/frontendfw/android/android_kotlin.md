En el pasado, todo era Java, ahora es Kotlin y es muy diferente a lo que recordabas.

## ¿Qué es una Actividad?

Una Activity es una vista en una aplicación Android, la interfaz visual donde users pueden realizar acciones, operar con la app. Cada pantalla, cada vista, cada página de una app, es una Activity.

El punto de entrada de una aplicación será una Activity.

Deben estar declaradas en el Manifest de la aplicación, se define con ficheros XML y se opera con partial class en Java o Kotlin.

Los diferentes componentes dentro de una view son views o widgets.

### Definir Activity parent

```xml
<activity
    android:name=".DetalleActivity"
    android:parentActivityName=".ListaActivity">
    <!-- Otras configuraciones de la actividad -->
</activity>
```

### Activity lifecycle:

Estas tienen diferentes estados en su ciclo de vida:

![Activity lifecycle](https://developer.android.com/guide/components/images/activity_lifecycle.png?hl=es-419)

Sobre el ciclo de vida de una Activity:

1. **onCreate()**: Se llama cuando la Activity se crea por primera vez. Aquí es donde debes inicializar tu Activity, como configurar la interfaz de usuario e inicializar variables.
2. **onStart()**: Se llama cuando la Activity se vuelve visible para el usuario.
3. **onResume()**: Se llama cuando la Activity comienza a interactuar con el usuario. En este punto, la Activity está en la parte superior de la pila de Activities.
4. **onPause()**: Se llama cuando el sistema está a punto de reanudar otra Activity. Aquí es donde debes pausar tareas en curso, como animaciones o reproducción de música.
5. **onStop()**: Se llama cuando la Activity ya no es visible para el usuario.
6. **onDestroy()**: Se llama antes de que la Activity sea destruida. Esta es la última llamada que recibe la Activity.
7. **onRestart()**: Se llama después de que la Activity ha sido detenida, justo antes de que se inicie nuevamente.

Estos métodos son sobreescribles para capturar estos momentos específicos del ciclo de vida de una app e interactuar sobre ellos.

Dependiendo de la acción que se realice sobre el dispositivo físico, también se dan eventos a tener en cuenta: 

![[android_events_navigation.png]]
## ¿Qué es Gradle?

**[Gradle](https://github.com/Beelzenef/AndroidMe/blob/gh-pages/kno/t1.md#buildgradle)** es un sistema de compilación utilizado en **Android Studio**. Es un **plugin**, lo que facilita su actualización y exportación entre proyectos. Permite la reutilización de código, configuración personalizada y gestión de dependencias de forma potente. Automatiza procesos de compilación y evita errores comunes.

ADB (Android Debug Bridge) nos permite realizar las siguientes tareas:

* instalar APKs
* manejar el estado de los dispositivos a los que tenemos acceso (ya sean físicos o emuladores)
* gestionar ficheros dentro de esos ficheros

## Layouts

* ConstraintLayout
* LinearLayout
* RelativeLayout
* FrameLayout
* GridLayout

## Manejando views (widgets/controles)

En el pasado, operábamos con `findViewById`, pero en la actualidad existe el `viewBinding`, que genera el código de nuestras vistas con nullsafety y asegurando los tipos.

En el fichero build.gradle:

```groovy
android {
    ...
    buildFeatures {
        viewBinding true
    }
}
```

Y en nuestra vista para Page:

```kotlin
private lateinit var binding: ResultProfileBinding
override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    binding = ResultProfileBinding.inflate(layoutInflater)
    setContentView(binding.root)
}
```

Este sistema sigue requiriendo de dar un ID a cada uno de las views que vayamos a incluir, porque será el nombre de la variable que se generará en este `viewBinding`.

## Login

### EditText

```xml
	<EditText
        android:id="@+id/etEmail"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="Correo electrónico"
        android:inputType="textEmailAddress"/>

    <EditText
        android:id="@+id/etPassword"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="Contraseña"
        android:inputType="textPassword"/>

    <Button
        android:id="@+id/btnLogin"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Iniciar sesión"/>
```

### TextInputLayout

```xml
	<com.google.android.material.textfield.TextInputLayout
	    android:id="@+id/tilEmail"
	    android:layout_width="match_parent"
	    android:layout_height="wrap_content"
	    android:hint="Correo electrónico">
	    <com.google.android.material.textfield.TextInputEditText
	        android:id="@+id/etEmail"
	        android:layout_width="match_parent"
	        android:layout_height="wrap_content"
	        android:inputType="textEmailAddress"/>
	</com.google.android.material.textfield.TextInputLayout>

	<com.google.android.material.textfield.TextInputLayout
	    android:id="@+id/tilPassword"
	    android:layout_width="match_parent"
	    android:layout_height="wrap_content"
	    android:hint="Contraseña">

    <com.google.android.material.textfield.TextInputEditText
        android:id="@+id/etPassword"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:inputType="textPassword"/>
```

## SnackBars y Toasts

```kotlin
val button = findViewById<Button>(R.id.btnSnackBar)
        button.setOnClickListener { view ->
            val snackBar = Snackbar.make(view, "Reemplaza con tu propia acción", Snackbar.LENGTH_LONG)
                .setAction("Acción") {
                    // Acción al hacer clic en el botón de la Snackbar
                    Toast.makeText(this, "Acción realizada", Toast.LENGTH_SHORT).show()
                }
            snackBar.show()
```

Switch

```xml
<Switch
    android:id="@+id/simpleSwitch"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="Switch" />
```

```kotlin
val simpleSwitch: Switch = findViewById(R.id.simpleSwitch)
simpleSwitch.setOnCheckedChangeListener { _, isChecked ->
    if (isChecked) {
        Toast.makeText(applicationContext, "Switch ON", Toast.LENGTH_SHORT).show()
    } else {
        Toast.makeText(applicationContext, "Switch OFF", Toast.LENGTH_SHORT).show()
    }
}
```

ImageView

```xml
<Switch
    android:id="@+id/simpleSwitch"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="Switch" />

<ImageView
    android:id="@+id/imageView"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:src="@drawable/image1" />
```

```kotlin
override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        binding = ActivityMainBinding.inflate(layoutInflater)
        setContentView(binding.root)

        binding.simpleSwitch.setOnCheckedChangeListener { _, isChecked ->
            if (isChecked) {
                binding.imageView.setImageResource(R.drawable.image2)
            } else {
                binding.imageView.setImageResource(R.drawable.image1)
            }
        }
    }
```

Tipos de imagenes

Conversores a vectores

Cargar una imagen de la red con CoilkT

MutableLiveData
LiveData

Trabajando con viewModels
activity-ktx (library)

## Intents

### Explicit

* Especifica qué app manejará el intent
* Provee el nombre del package o componente

### Implicit

* No se espeficica app concreta
* Declara una acción general a ejecutar
* Se invoca cuando necesitas que otra app ejecute una acción concreta

Por ejemplo: abrir la agenda para marcar un número de teléfono, abrir un mapa interactivo (Google Maps)

¿Cómo se diferencian?

Con intent filters

Marcar un intent-filter para determinar una Activity como main y launcher, en comparación con otras:

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.myapp">

    <uses-permission android:name="android.permission.INTERNET" />

    <!-- Activity principal -->
    <application>
        <activity android:name=".MainActivity">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>

        <activity android:name=".SecondActivity" />
        <activity android:name=".DetailActivity" />
    </application>
</manifest>
```
#### Intent explicit

Para abrir una Activity llamada MainActivity

```kotlin
class EntryActivity : AppCompatActivity() {

    private lateinit var binding: ActivityEntryBinding

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        binding = ActivityEntryBinding.inflate(layoutInflater)
        setContentView(binding.root)

        binding.enterButton.setOnClickListener {
            val intent = Intent(this, MainActivity::class.java)
            startActivity(intent)
        }
    }
}
```

#### Intents implicitos

Para enviar SMS:

```kotlin
val smsButton = findViewById<Button>(R.id.smsButton)
smsButton.setOnClickListener {
    val intent = Intent(Intent.ACTION_SENDTO)
    intent.data = Uri.parse("smsto:123456789") 
    intent.putExtra("sms_body", "¡Hola desde mi app!") 
    startActivity(intent)
}
```

Para hacer una llamada telefónica:

```kotlin
val callButton = findViewById<Button>(R.id.callButton)
callButton.setOnClickListener {
    val intent = Intent(Intent.ACTION_DIAL)
    intent.data = Uri.parse("tel:123456789")
    startActivity(intent)
}
```

Para enviar un email:

```kotlin
val emailButton = findViewById<Button>(R.id.emailButton)
emailButton.setOnClickListener {
    val intent = Intent(Intent.ACTION_SENDTO)
    intent.data = Uri.parse("mailto:correo@example.com")
    intent.putExtra(Intent.EXTRA_SUBJECT, "Asunto del correo")
    intent.putExtra(Intent.EXTRA_TEXT, "Contenido del correo")
    startActivity(intent)
}
```

Para abrir un mapa, con coordenadas:

```kotlin
val mapButton = findViewById<Button>(R.id.mapButton)
mapButton.setOnClickListener {
    val intent = Intent(Intent.ACTION_VIEW)
    intent.data = Uri.parse("geo:37.7749,-122.4194")
    startActivity(intent)
}
```