## what is

Framework open-source creado por Google, mantenido por Google y la comunidad, para hacer software crossplatform ([[android_kotlin]], iOS, Windows, Linux, Mac y web)

## Accesibilidad en Flutter

Cada framework o herramienta tiene sus propias formas de aplicar [[accesibilidad]]

### accesibility_tools

[accessibility_tools | Flutter package (pub.dev)](https://pub.dev/packages/accessibility_tools)

## Áreas donde poner el foco

### readability

text size

* Separa el contenido para dar claridad
* Muestra el contenido esencial en primer lugar, el orden cuenta
* Usa fuente legible con tamaño adecuado
* Usa espaciado de caracteres adecuado (kerning)
* Usa espaciado de líneas adecuado
* En general: **usa el espacio que tienes disponible**

Text resizer (Figma plugin)

#### Text overflow

```dart
// bad
Row(
	children: [
		Icon(Icons.person),
		child: Text('Firstname Lastname')
	]
)

// good
Row(
	children: [
		Icon(Icons.person),
		Expanded(child: Text('Firstname Lastname'))
	]
)
```

#### Wrap vs Row

```dart
// good
Row(
	children: [
		Expanded(child: CheckBoxListTile(title: Text(''))),
		Expanded(child: CheckBoxListTile(title: Text(''))),
	]
)

// good
Wrap(
	children: [
		Expanded(child: CheckBoxListTile(title: Text(''))),
		Expanded(child: CheckBoxListTile(title: Text(''))),
	]
)
```

Texto recortado por falta de espacio...

```dart
Text('Esto es un texto gigantesco...',
	 maxLines: 3,
	overflow: TextOverflow.ellipsis)

//...

//media query??
```
### input

### 