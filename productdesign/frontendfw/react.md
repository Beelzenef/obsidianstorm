## Fragments

Los Fragments en React permiten agrupar múltiples elementos sin añadir nodos extra al DOM. Esto es útil para mantener un DOM limpio.

De esta forma, cumplimos con la norma en React en la que un componente solo puede devolver un elemento HTML (con n elementos anidados).

```jsx
import React from 'react';

function Example() {
    return (
        <>
            <h1>Título</h1>
            <p>Este es un párrafo.</p>
        </>
    );
}

export default Example;
```

## Props

Los Props (propiedades) son datos que se pasan de un componente padre a un componente hijo. Son inmutables y permiten personalizar el comportamiento o la apariencia de los componentes.

```jsx
import React from 'react';

function Greeting({ name }) {
    return <h1>Hola, {name}!</h1>;
}

function App() {
    return <Greeting name="Juan" />;
}

export default App;
```

## PropTypes y DefaultProps

**PropTypes** se utilizan para validar las propiedades que un componente recibe. Esto ayuda a garantizar que los datos pasados a los componentes sean del tipo esperado.

**DefaultProps** permite definir valores predeterminados para las propiedades en caso de que no se proporcionen.

```jsx
import React from 'react';
import PropTypes from 'prop-types';

function Greeting({ name, age }) {
    return (
        <div>
            <h1>Hola, {name}!</h1>
            <p>Tienes {age} años.</p>
        </div>
    );
}

Greeting.propTypes = {
    name: PropTypes.string.isRequired,
    age: PropTypes.number,
};

Greeting.defaultProps = {
    age: 18,
};

function App() {
    return <Greeting name="Juan" />;
}

export default App;
```

## Sobre hooks

Son funciones especiales que permiten utilizar el estado y otras características de React sin necesidad de escribir clases. Introducidos en React 16.8, han revolucionado la forma de manejar la lógica y los efectos en los componentes funcionales.

Aquí tienes algunos de los Hooks más importantes:

- `useState`: Permite añadir estado local a los componentes funcionales.
- `useEffect`: Se usa para manejar efectos secundarios, como llamadas a APIs o manipulación del DOM.
- `useContext`: Facilita el acceso a valores dentro de un contexto sin necesidad de pasar props manualmente.
- `useRef`: Permite mantener referencias a elementos del DOM o valores que persisten entre renders sin causar re-renderizaciones.
- `useMemo` y `useCallback`: Optimizan el rendimiento al evitar cálculos o funciones innecesarias.

Cada uno de estos Hooks tiene su propia utilidad y pueden combinarse para crear lógica compleja dentro de componentes funcionales.

### `useState`

`useState` permite manejar el estado dentro de un componente funcional.

Se usa para almacenar, get y set de valores de cualquier tipo. Por ejemplo:

```jsx
import React, { useState } from "react";

function Contador() {
  const [contador, setContador] = useState(0);

  return (
    <div>
      <p>Has hecho clic {contador} veces.</p>
      <button onClick={() => setContador(contador + 1)}>Incrementar</button>
    </div>
  );
}

export default Contador;
```

Cada vez que el usuario presiona el botón, `setContador` actualiza el estado y React re-renderiza el componente.

### `useEffect`

`useEffect` permite manejar efectos secundarios como llamadas a APIs.

Se usa para ejecutar código bajo condiciones o eventos determinados, aquellos determinados por sus dependencias. No permite llamadas asíncronas en su interior de forma directa, pero sí podemos llamar a una función async que esté declarada fuera del hook (pero sin await).

```jsx
import React, { useState, useEffect } from "react";

function Usuarios() {
  const [usuarios, setUsuarios] = useState([]);

  useEffect(() => {
    fetch("https://jsonplaceholder.typicode.com/users")
      .then((response) => response.json())
      .then((data) => setUsuarios(data));
  }, []);

  return (
    <ul>
      {usuarios.map((usuario) => (
        <li key={usuario.id}>{usuario.name}</li>
      ))}
    </ul>
  );
}

export default Usuarios;
```

Aquí, la función fetch obtiene datos cuando el componente se monta.

### `useContext`

`useContext` permite acceder a valores globales sin pasar props manualmente:

```jsx
import React, { useContext, createContext } from "react";

const TemaContext = createContext("light");

function Boton() {
  const tema = useContext(TemaContext);
  return <button style={{ backgroundColor: tema === "light" ? "#fff" : "#333" }}>Botón</button>;
}

function App() {
  return (
    <TemaContext.Provider value="dark">
      <Boton />
    </TemaContext.Provider>
  );
}

export default App;
```

Aquí el botón cambia su estilo basado en el valor del contexto sin recibir props explícitamente.

### `useRef`

`useRef` permite acceder a elementos del DOM sin causar re-renderizaciones:

```jsx
import React, { useRef } from "react";

function InputFoco() {
  const inputRef = useRef(null);

  const manejarClick = () => {
    inputRef.current.focus();
  };

  return (
    <div>
      <input ref={inputRef} type="text" />
      <button onClick={manejarClick}>Focalizar Input</button>
    </div>
  );
}

export default InputFoco;
```

Aquí `useRef` mantiene una referencia al input y permite enfocarlo sin afectar el estado.

### `useMemo`

`useMemo` optimiza cálculos costosos para evitar recomputaciones innecesarias:

```jsx
import React, { useState, useMemo } from "react";

function Calculo() {
  const [contador, setContador] = useState(0);
  const resultado = useMemo(() => contador * 10, [contador]);

  return (
    <div>
      <p>Resultado calculado: {resultado}</p>
      <button onClick={() => setContador(contador + 1)}>Incrementar</button>
    </div>
  );
}

export default Calculo;
```

Aquí, resultado solo se recalcula cuando contador cambia.

### `useCallback`

`useCallback` memoriza funciones para evitar su re-creación en cada render:

```jsx
import React, { useState, useCallback } from "react";

function Contador() {
  const [contador, setContador] = useState(0);

  const incrementar = useCallback(() => {
    setContador((prev) => prev + 1);
  }, []);

  return (
    <div>
      <p>Contador: {contador}</p>
      <button onClick={incrementar}>Incrementar</button>
    </div>
  );
}

export default Contador;
```

Aquí, la función incrementar no se recrea en cada render, mejorando el rendimiento.

Estos son ejemplos básicos, pero los Hooks pueden combinarse para crear soluciones más avanzadas.

### `useReducer`

`useReducer` es un Hook que proporciona una alternativa más poderosa a useState, especialmente cuando el estado es complejo y requiere lógica más estructurada. Se basa en el patrón de reducer utilizado en Redux, donde el estado se actualiza mediante una función "reductora".

#### Cómo funciona

`useReducer` toma dos argumentos principales:

- Una función reducer, que recibe el estado actual y una acción, y devuelve un nuevo estado.
- Un estado inicial.

También devuelve un array con:

- El estado actual.
- Una función dispatch para enviar acciones al reducer.

#### Ejemplo básico

Imagina que quieres manejar el estado de un contador con acciones explícitas:

```jsx
import React, { useReducer } from "react";

const initialState = { contador: 0 };

function reducer(state, action) {
  switch (action.type) {
    case "incrementar":
      return { contador: state.contador + 1 };
    case "decrementar":
      return { contador: state.contador - 1 };
    case "reset":
      return initialState;
    default:
      return state;
  }
}

function Contador() {
  const [state, dispatch] = useReducer(reducer, initialState);

  return (
    <div>
      <p>Contador: {state.contador}</p>
      <button onClick={() => dispatch({ type: "incrementar" })}>+</button>
      <button onClick={() => dispatch({ type: "decrementar" })}>-</button>
      <button onClick={() => dispatch({ type: "reset" })}>Reset</button>
    </div>
  );
}

export default Contador;
```

#### Ventajas de useReducer sobre useState

- Es más estructurado y escalable cuando hay múltiples valores en el estado.
- Facilita la gestión de lógica compleja en grandes aplicaciones.
- Reduce errores al centralizar la lógica de actualización del estado en el reducer.

Se utiliza mucho en aplicaciones donde el estado tiene múltiples sub-valores o cuando las actualizaciones dependen de acciones específicas. 

---

Cada framework o herramienta tiene sus propias formas de aplicar [[accesibilidad]]