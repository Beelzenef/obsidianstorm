## Fragments

Los Fragments en React permiten agrupar múltiples elementos sin añadir nodos extra al DOM. Esto es útil para mantener un DOM limpio y evitar elementos `<div>` innecesarios que pueden afectar el CSS o la semántica HTML.

Aunque desde React 16 los componentes pueden devolver arrays, strings, números o fragments, los Fragments son la solución más elegante cuando necesitas devolver múltiples elementos JSX sin un contenedor adicional.

```tsx
import React from 'react';

function Example(): JSX.Element {
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

```tsx
import React from 'react';

interface GreetingProps {
    name: string;
}

function Greeting({ name }: GreetingProps): JSX.Element {
    return <h1>Hola, {name}!</h1>;
}

function App(): JSX.Element {
    return <Greeting name="Juan" />;
}

export default App;
```

## PropTypes y DefaultProps

**PropTypes** se utilizan para validar las propiedades que un componente recibe. Esto ayuda a garantizar que los datos pasados a los componentes sean del tipo esperado.

**⚠️ Nota importante**: PropTypes fue separado de React en la versión 15.5 y ahora debe instalarse como paquete independiente. En proyectos modernos, se recomienda usar **TypeScript** para validación de tipos, que proporciona mejor autocompletado y detección de errores en tiempo de compilación.

**DefaultProps** permite definir valores predeterminados para las propiedades en caso de que no se proporcionen.

```tsx
import React from 'react';
import PropTypes from 'prop-types';

interface GreetingProps {
    name: string;
    age?: number;
}

function Greeting({ name, age }: GreetingProps): JSX.Element {
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

function App(): JSX.Element {
    return <Greeting name="Juan" />;
}

export default App;
```

### Alternativa moderna con TypeScript

```tsx
interface GreetingProps {
    name: string;
    age?: number; // Opcional con valor por defecto
}

function Greeting({ name, age = 18 }: GreetingProps) {
    return (
        <div>
            <h1>Hola, {name}!</h1>
            <p>Tienes {age} años.</p>
        </div>
    );
}

function App() {
    return <Greeting name="Juan" />;
}

export default App;
```

## Reglas de los Hooks

Los Hooks tienen reglas específicas que **siempre** debes seguir:

1. **Solo llama Hooks en el nivel superior**: Nunca dentro de loops, condiciones o funciones anidadas
2. **Solo llama Hooks desde componentes React**: O desde otros custom hooks
3. **Usa el prefijo "use"** para custom hooks

```tsx
// ❌ INCORRECTO
interface ComponenteMaloProps {
    mostrar: boolean;
}

function ComponenteMalo({ mostrar }: ComponenteMaloProps): JSX.Element {
    if (mostrar) {
        const [estado, setEstado] = useState<number>(0); // ¡Error!
    }
    
    for (let i = 0; i < 3; i++) {
        useEffect(() => {}); // ¡Error!
    }
    
    return <div>Malo</div>;
}

// ✅ CORRECTO
interface ComponenteBuenoProps {
    mostrar: boolean;
}

function ComponenteBueno({ mostrar }: ComponenteBuenoProps): JSX.Element {
    const [estado, setEstado] = useState<number>(0);
    
    useEffect(() => {
        if (mostrar) {
            // Lógica condicional dentro del hook
        }
    }, [mostrar]);
    
    return <div>Bueno</div>;
}
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

```tsx
import React, { useState } from "react";

function Contador(): JSX.Element {
  const [contador, setContador] = useState<number>(0);

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

`useEffect` permite manejar efectos secundarios como llamadas a APIs, suscripciones, timers, o manipulación directa del DOM.

Se ejecuta después de cada render por defecto, pero puedes controlar cuándo se ejecuta usando el **array de dependencias**:

- **Sin array de dependencias**: Se ejecuta después de cada render
- **Array vacío `[]`**: Se ejecuta solo una vez (al montar el componente)
- **Con dependencias `[dep1, dep2]`**: Se ejecuta cuando cambian las dependencias

#### Patrón básico con limpieza

```tsx
import React, { useState, useEffect } from "react";

function Temporizador(): JSX.Element {
  const [segundos, setSegundos] = useState<number>(0);

  useEffect(() => {
    // Configurar el timer
    const interval = setInterval(() => {
      setSegundos(prev => prev + 1);
    }, 1000);

    // Función de limpieza (cleanup)
    return () => {
      clearInterval(interval); // ¡Importante para evitar memory leaks!
    };
  }, []); // Array vacío = solo se ejecuta al montar

  return <div>Segundos: {segundos}</div>;
}

export default Temporizador;
```

#### Ejemplo con llamada a API

```tsx
import React, { useState, useEffect } from "react";

interface Usuario {
  id: number;
  name: string;
  email: string;
}

function Usuarios(): JSX.Element {
  const [usuarios, setUsuarios] = useState<Usuario[]>([]);
  const [loading, setLoading] = useState<boolean>(true);
  const [error, setError] = useState<string | null>(null);

  useEffect(() => {
    // Función async separada dentro del useEffect
    async function fetchUsuarios(): Promise<void> {
      try {
        const response = await fetch("https://jsonplaceholder.typicode.com/users");
        if (!response.ok) throw new Error('Error al cargar usuarios');
        const data: Usuario[] = await response.json();
        setUsuarios(data);
      } catch (err) {
        setError(err instanceof Error ? err.message : 'Error desconocido');
      } finally {
        setLoading(false);
      }
    }

    fetchUsuarios();
  }, []); // Se ejecuta solo una vez

  if (loading) return <div>Cargando...</div>;
  if (error) return <div>Error: {error}</div>;

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

#### Equivalencias con ciclo de vida de clases

```tsx
useEffect(() => {
  // componentDidMount + componentDidUpdate
  console.log('Componente montado o actualizado');
});

useEffect(() => {
  // componentDidMount solamente
  console.log('Componente montado');
}, []);

useEffect(() => {
  // componentDidUpdate solo cuando cambia 'contador'
  console.log('Contador cambió');
}, [contador]);

useEffect(() => {
  return () => {
    // componentWillUnmount
    console.log('Componente se va a desmontar');
  };
}, []);
```

### `useContext`

`useContext` permite acceder a valores globales sin pasar props manualmente:

```tsx
import React, { useContext, createContext } from "react";

type Theme = "light" | "dark";

const TemaContext = createContext<Theme>("light");

function Boton(): JSX.Element {
  const tema = useContext(TemaContext);
  return (
    <button style={{ backgroundColor: tema === "light" ? "#fff" : "#333" }}>
      Botón
    </button>
  );
}

function App(): JSX.Element {
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

```tsx
import React, { useRef } from "react";

function InputFoco(): JSX.Element {
  const inputRef = useRef<HTMLInputElement>(null);

  const manejarClick = (): void => {
    inputRef.current?.focus();
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

```tsx
import React, { useState, useMemo } from "react";

function Calculo(): JSX.Element {
  const [contador, setContador] = useState<number>(0);
  const resultado = useMemo((): number => contador * 10, [contador]);

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

`useCallback` memoriza funciones para evitar su re-creación en cada render. Es especialmente útil cuando pasas funciones como props a componentes hijos que están optimizados con `React.memo`.

#### Ejemplo real donde useCallback es necesario

```tsx
import React, { useState, useCallback, memo } from "react";

// Componente hijo optimizado con memo
interface BotonOptimizadoProps {
  onClick: () => void;
  children: React.ReactNode;
}

const BotonOptimizado = memo(({ onClick, children }: BotonOptimizadoProps): JSX.Element => {
  console.log('BotonOptimizado se renderizó'); // Para demostrar re-renders
  return <button onClick={onClick}>{children}</button>;
});

function ContadorConBoton(): JSX.Element {
  const [contador, setContador] = useState<number>(0);
  const [otroEstado, setOtroEstado] = useState<number>(0);

  // ❌ Sin useCallback: la función se recrea en cada render
  const incrementarMal = (): void => {
    setContador(prev => prev + 1);
  };

  // ✅ Con useCallback: la función se memoriza
  const incrementarBien = useCallback((): void => {
    setContador(prev => prev + 1);
  }, []); // Sin dependencias porque usa la forma funcional de setState

  return (
    <div>
      <p>Contador: {contador}</p>
      <p>Otro estado: {otroEstado}</p>
      
      {/* Este botón se re-renderiza innecesariamente */}
      <BotonOptimizado onClick={incrementarMal}>
        Incrementar (malo)
      </BotonOptimizado>
      
      {/* Este botón NO se re-renderiza cuando cambia otroEstado */}
      <BotonOptimizado onClick={incrementarBien}>
        Incrementar (bueno)
      </BotonOptimizado>
      
      <button onClick={() => setOtroEstado(prev => prev + 1)}>
        Cambiar otro estado
      </button>
    </div>
  );
}

export default ContadorConBoton;
```

#### Cuándo usar useCallback

- Cuando pasas funciones como props a componentes optimizados con `memo`
- En dependencias de otros hooks como `useEffect`
- Para funciones que son costosas de crear

#### Cuándo NO usar useCallback

- Para funciones simples que no se pasan como props
- Cuando las dependencias cambian constantemente
- En la mayoría de casos (úsalo solo cuando hay problemas de rendimiento reales)

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

```tsx
import React, { useReducer } from "react";

interface State {
  contador: number;
}

type Action = 
  | { type: "incrementar" }
  | { type: "decrementar" }
  | { type: "reset" };

const initialState: State = { contador: 0 };

function reducer(state: State, action: Action): State {
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

function Contador(): JSX.Element {
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

#### Cuándo usar useReducer vs useState

**Usa `useReducer` cuando:**

- El estado tiene múltiples sub-valores relacionados
- La lógica de actualización es compleja
- Necesitas acciones específicas y controladas
- El estado depende del estado anterior de manera compleja
- Compartes lógica de estado entre componentes

**Usa `useState` cuando:**

- El estado es simple (primitivos, arrays simples)
- Las actualizaciones son directas
- No hay lógica compleja de transiciones

#### Ejemplo más avanzado: Formulario

```tsx
import React, { useReducer } from "react";

interface FormState {
  nombre: string;
  email: string;
  errors: Record<string, string | null>;
  isSubmitting: boolean;
}

type FormAction = 
  | { type: 'SET_FIELD'; field: keyof Pick<FormState, 'nombre' | 'email'>; value: string }
  | { type: 'SET_ERROR'; field: string; error: string }
  | { type: 'SET_SUBMITTING'; value: boolean }
  | { type: 'RESET_FORM' };

const initialState: FormState = {
  nombre: '',
  email: '',
  errors: {},
  isSubmitting: false
};

function formReducer(state: FormState, action: FormAction): FormState {
  switch (action.type) {
    case 'SET_FIELD':
      return {
        ...state,
        [action.field]: action.value,
        errors: { ...state.errors, [action.field]: null } // Limpiar error
      };
    
    case 'SET_ERROR':
      return {
        ...state,
        errors: { ...state.errors, [action.field]: action.error }
      };
    
    case 'SET_SUBMITTING':
      return { ...state, isSubmitting: action.value };
    
    case 'RESET_FORM':
      return initialState;
    
    default:
      return state;
  }
}

function FormularioComplejo(): JSX.Element {
  const [state, dispatch] = useReducer(formReducer, initialState);

  const handleSubmit = async (e: React.FormEvent<HTMLFormElement>): Promise<void> => {
    e.preventDefault();
    dispatch({ type: 'SET_SUBMITTING', value: true });
    
    try {
      // Simular envío
      await new Promise(resolve => setTimeout(resolve, 1000));
      dispatch({ type: 'RESET_FORM' });
    } catch (error) {
      dispatch({ type: 'SET_ERROR', field: 'general', error: 'Error al enviar' });
    } finally {
      dispatch({ type: 'SET_SUBMITTING', value: false });
    }
  };

  return (
    <form onSubmit={handleSubmit}>
      <input
        value={state.nombre}
        onChange={(e) => dispatch({ 
          type: 'SET_FIELD', 
          field: 'nombre', 
          value: e.target.value 
        })}
        placeholder="Nombre"
      />
      {state.errors.nombre && <span>{state.errors.nombre}</span>}
      
      <input
        value={state.email}
        onChange={(e) => dispatch({ 
          type: 'SET_FIELD', 
          field: 'email', 
          value: e.target.value 
        })}
        placeholder="Email"
      />
      {state.errors.email && <span>{state.errors.email}</span>}
      
      <button disabled={state.isSubmitting}>
        {state.isSubmitting ? 'Enviando...' : 'Enviar'}
      </button>
    </form>
  );
}

export default FormularioComplejo;
```

Se utiliza mucho en aplicaciones donde el estado tiene múltiples sub-valores o cuando las actualizaciones dependen de acciones específicas.

## Custom Hooks

Los Custom Hooks permiten extraer lógica de componentes y reutilizarla. Son funciones que usan otros hooks y siguen las reglas de los hooks.

### Ejemplo: Hook para llamadas a API

```tsx
import { useState, useEffect } from 'react';

interface UseFetchResult<T> {
  data: T | null;
  loading: boolean;
  error: string | null;
}

// Custom hook para fetch de datos
function useFetch<T>(url: string): UseFetchResult<T> {
  const [data, setData] = useState<T | null>(null);
  const [loading, setLoading] = useState<boolean>(true);
  const [error, setError] = useState<string | null>(null);

  useEffect(() => {
    async function fetchData(): Promise<void> {
      try {
        setLoading(true);
        const response = await fetch(url);
        if (!response.ok) throw new Error('Error en la petición');
        const result: T = await response.json();
        setData(result);
      } catch (err) {
        setError(err instanceof Error ? err.message : 'Error desconocido');
      } finally {
        setLoading(false);
      }
    }

    fetchData();
  }, [url]);

  return { data, loading, error };
}

// Uso del custom hook
interface Usuario {
  id: number;
  name: string;
  email: string;
}

function ListaUsuarios(): JSX.Element {
  const { data: usuarios, loading, error } = useFetch<Usuario[]>('/api/usuarios');

  if (loading) return <div>Cargando...</div>;
  if (error) return <div>Error: {error}</div>;

  return (
    <ul>
      {usuarios?.map(usuario => (
        <li key={usuario.id}>{usuario.name}</li>
      ))}
    </ul>
  );
}
```

### Ejemplo: Hook para localStorage

```tsx
import { useState } from 'react';

type UseLocalStorageReturn<T> = [T, (value: T | ((val: T) => T)) => void];

function useLocalStorage<T>(key: string, initialValue: T): UseLocalStorageReturn<T> {
  // Obtener valor inicial del localStorage
  const [storedValue, setStoredValue] = useState<T>(() => {
    try {
      const item = window.localStorage.getItem(key);
      return item ? JSON.parse(item) : initialValue;
    } catch (error) {
      console.error('Error al leer localStorage:', error);
      return initialValue;
    }
  });

  // Función para actualizar el valor
  const setValue = (value: T | ((val: T) => T)): void => {
    try {
      // Permitir que value sea una función como en useState
      const valueToStore = value instanceof Function ? value(storedValue) : value;
      setStoredValue(valueToStore);
      window.localStorage.setItem(key, JSON.stringify(valueToStore));
    } catch (error) {
      console.error('Error al escribir en localStorage:', error);
    }
  };

  return [storedValue, setValue];
}

// Uso del custom hook
function ComponenteConPersistencia(): JSX.Element {
  const [contador, setContador] = useLocalStorage<number>('contador', 0);

  return (
    <div>
      <p>Contador persistente: {contador}</p>
      <button onClick={() => setContador(c => c + 1)}>
        Incrementar
      </button>
      <button onClick={() => setContador(0)}>
        Reset
      </button>
    </div>
  );
}
```

## Error Boundaries

Los Error Boundaries capturan errores JavaScript en cualquier lugar del árbol de componentes hijo y muestran una UI de respaldo.

**Nota**: Los Error Boundaries solo funcionan con componentes de clase. Para componentes funcionales, puedes usar librerías como `react-error-boundary`.

### Implementación con clase

```tsx
import React from 'react';

interface ErrorBoundaryState {
  hasError: boolean;
  error: Error | null;
}

interface ErrorBoundaryProps {
  children: React.ReactNode;
}

class ErrorBoundary extends React.Component<ErrorBoundaryProps, ErrorBoundaryState> {
  constructor(props: ErrorBoundaryProps) {
    super(props);
    this.state = { hasError: false, error: null };
  }

  static getDerivedStateFromError(error: Error): ErrorBoundaryState {
    // Actualiza el state para mostrar la UI de error
    return { hasError: true, error };
  }

  componentDidCatch(error: Error, errorInfo: React.ErrorInfo): void {
    // Puedes registrar el error en un servicio de logging
    console.error('Error capturado por Error Boundary:', error, errorInfo);
  }

  render(): React.ReactNode {
    if (this.state.hasError) {
      return (
        <div style={{ padding: '20px', textAlign: 'center' }}>
          <h2>¡Algo salió mal!</h2>
          <p>Ha ocurrido un error inesperado.</p>
          <button onClick={() => this.setState({ hasError: false, error: null })}>
            Intentar de nuevo
          </button>
        </div>
      );
    }

    return this.props.children;
  }
}

// Uso
function App(): JSX.Element {
  return (
    <ErrorBoundary>
      <ComponenteQuePuedeRomper />
    </ErrorBoundary>
  );
}
```

### Con react-error-boundary (recomendado)

```tsx
import { ErrorBoundary } from 'react-error-boundary';

interface ErrorFallbackProps {
  error: Error;
  resetErrorBoundary: () => void;
}

function ErrorFallback({ error, resetErrorBoundary }: ErrorFallbackProps): JSX.Element {
  return (
    <div role="alert">
      <h2>¡Algo salió mal!</h2>
      <pre>{error.message}</pre>
      <button onClick={resetErrorBoundary}>Intentar de nuevo</button>
    </div>
  );
}

function App(): JSX.Element {
  return (
    <ErrorBoundary
      FallbackComponent={ErrorFallback}
      onError={(error: Error, errorInfo: {componentStack: string}) => {
        console.error('Error registrado:', error, errorInfo);
      }}
      onReset={() => {
        // Lógica adicional al resetear
        console.log('Error boundary reseteado');
      }}
    >
      <ComponenteQuePuedeRomper />
    </ErrorBoundary>
  );
}
```

**Los Error Boundaries NO capturan:**
- Errores en event handlers
- Errores en código asíncrono
- Errores durante el renderizado en server-side
- Errores en el propio Error Boundary 

---

Cada framework o herramienta tiene sus propias formas de aplicar [[accesibilidad]]