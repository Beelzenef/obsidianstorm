En el pasado, todo era Java, ahora es Kotlin y es muy diferente a lo que recordabas.

## ¿Qué es una Actividad?

Una Activity es una vista en una aplicación Android, la interfaz visual donde users pueden realizar acciones, operar con la app. Cada pantalla, cada vista, cada página de una app, es una Activity.

El punto de entrada de una aplicación será una Activity.

Estas tienen diferentes estados en su ciclo de vida:

![Activity lifecycle](https://developer.android.com/guide/components/images/activity_lifecycle.png?hl=es-419)

Deben estar declaradas en el Manifest de la aplicación, se define con ficheros XML y se opera con partial class en Java o Kotlin.

Los diferentes componentes dentro de una view son views o widgets.

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