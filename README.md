# Calculadora Científica 

Esta calculadora científica fue diseñada usando el entorno gráfico de Java `JFrame` y es capaz de realizar operaciones aritméticas básicas (suma, resta, multiplicación, división, potencia) y funciones científicas (seno, coseno, tangente, raíz cuadrada, factorial), así como manejar constantes como π (pi). Además, el código convierte expresiones de notación infija (ej. `3 + 4 * 2`) a notación postfija usando el algoritmo Shunting Yard para una evaluación precisa de la prioridad de operadores.

---

## Índice
1. **Clases y Métodos Principales**
2. **Conversión de Notación Infija a Postfija (`toPostfix` método)**
3. **Evaluación de Expresión Postfija (`evaluatePostfix` método)**
4. **Manejo de Funciones y Constantes Científicas**
5. **Manejo de Botones e Interfaz Gráfica**
6. **Pruebas y Casos de Uso**

---

## 1. Clases y Métodos Principales

La calculadora científica está contenida en una clase Java (`CalculadoraCientifica`), que gestiona tanto la interfaz gráfica como la lógica de cálculo. A continuación, se describen los métodos principales de la clase:

- **toPostfix(String expression):**  
  Convierte una expresión en notación infija a notación postfija para facilitar la evaluación posterior.
  
- **evaluatePostfix(ArrayList<String> tokens):**  
  Toma una lista de tokens en notación postfija y evalúa el resultado aplicando operadores y funciones correctamente.

- **isNumber(String token):**  
  Verifica si el token actual es un número.

- **isOperator(String token):**  
  Comprueba si el token es un operador aritmético (`+`, `-`, `*`, `/`, `^`).

- **isFunction(String token):**  
  Determina si el token es una función científica (`sin`, `cos`, `tan`, `sqrt`, `factorial`, `pi`).

---

## 2. Conversión de Notación Infija a Postfija (`toPostfix`)

Este método convierte la expresión del usuario de notación infija (ej. `3 + 4 * 2`) a notación postfija (ej. `3 4 2 * +`). Esta transformación permite que los operadores y las funciones se apliquen en el orden correcto durante la evaluación.

### Explicación
1. **Entrada de Tokens:**  
   Usa `StringTokenizer` para dividir la expresión en `+`, `-`, `*`, `/`, `^`, paréntesis y espacios. 

2. **Clasificación de Tokens:**  
   - **Números:** Se añaden directamente a la salida.
   - **Operadores:** Se añaden en función de su precedencia, manteniendo la prioridad de operaciones.
   - **Paréntesis:** Los paréntesis abren un nuevo contexto que cierra cuando aparece el paréntesis de cierre.
   - **Funciones:** Almacena las funciones en `operators`, tratándolas de modo similar a los operadores.

3. **Salida Final:**  
   Se vacía la pila de operadores y se agrega cada operador a la lista `output`, generando la expresión en notación postfija.

### Funcionamiento
El método toPostfix convierte una expresión matemática en notación infija a notación postfija utilizando el algoritmo de Shunting Yard de Dijkstra. Se usa una pila de operadores para manejar la precedencia y asociación de operadores. Los pasos son:

1. Si el token es un número, se agrega directamente a la salida.
2. Si el token es un operador, se desapilan operadores de mayor o igual precedencia antes de apilar el nuevo operador.
3. Si el token es un paréntesis izquierdo (, se apila.
4. Si el token es un paréntesis derecho ), se desapilan operadores hasta encontrar el paréntesis izquierdo correspondiente.
5. Si el token es una función matemática, se apila para procesarse más adelante.
6. Si el token es una constante (pi, e), se reemplaza por su valor numérico y se agrega a la salida.

Al final del recorrido, se vacía la pila de operadores en la salida.

### Ejemplo de Evaluación

#### Expresión en notación infija:
3 + sin(90) * 2

#### Conversión a Notación Postfija (toPostfix)

![image](https://github.com/user-attachments/assets/8ad74cc7-21d6-400f-b9ba-cce54d9830c9)

## 3. Evaluación de Expresión Postfija (`evaluatePostfix`)

Una vez que la expresión está en notación postfija, `evaluatePostfix` evalúa la expresión utilizando una pila (`stack`).

Descripción

El método evaluatePostfix evalúa una expresión matemática en notación postfija utilizando una pila (Stack).

### Funcionamiento

Este método recorre una lista de tokens (ArrayList<String> tokens) que representan una expresión postfija y la evalúa utilizando los siguientes pasos:

1. Si el token es un número, se convierte a double y se almacena en la pila.
2. Si el token es una función matemática, se extrae el último valor de la pila, se aplica la función y se almacena el resultado.
3. Si el token es un operador, se extraen los dos valores superiores de la pila, se realiza la operación y se almacena el resultado.
4. Al finalizar el recorrido, la pila contendrá el resultado de la expresión.

### Explicación
1. **Operadores y Números:**  
   Los números se apilan, mientras que los operadores toman dos valores de la pila y aplican la operación.

2. **Funciones Científicas:**  
   Las funciones toman un valor de la pila. Ejemplo: `sin` convierte grados a radianes y luego aplica `Math.sin`.

3. **Constantes:**  
   `pi` se añade a la pila directamente como `Math.PI`.

---

### Ejemplo de Evaluación

#### Expresión en notación infija: 
  3 + sin(90) * 2

#### Conversión a notación postfija:
  3 90 sin 2 * +
  
#### Paso a paso
![image](https://github.com/user-attachments/assets/c91e6542-98d8-487d-b3a2-c69a19cf20a2)

#### Resultado final:
5.0

 ## 4. Manejo de Funciones y Constantes Científicas

Para definir funciones como `sin`, `cos`, `tan`, `sqrt`, `factorial`, `pi`, hemos implementado los métodos `isFunction` y `evaluatePostfix`. Cada función científica tiene un caso específico en `evaluatePostfix`, aplicando la operación correcta o añadiendo `Math.PI` para `pi`.

```java
private boolean isFunction(String token) {
    return token.equals("sin") || token.equals("cos") || token.equals("tan") || 
           token.equals("sqrt") || token.equals("factorial") || token.equals("pi");
}
```

---

## 5. Manejo de Botones e Interfaz Gráfica

Cada botón en la interfaz gráfica se conecta a un método de evento (`ActionListener`) que agrega el carácter correspondiente al `display` o desencadena la evaluación. Los botones de operaciones científicas aplican `toPostfix` y `evaluatePostfix` a la expresión en el `display`.

---

## 6. Pruebas y Casos de Uso

Para verificar la funcionalidad:
1. **Operaciones Básicas:**  
   Prueba `2 + 3`, `5 * 6`, `8 / 4`, `2^3`.

2. **Funciones Científicas:**  
   Prueba `sin(90)`, `cos(0)`, `tan(45)`, `sqrt(16)`, `factorial(5)`.

3. **Casos Combinados:**  
   Prueba `3 + sin(90) * 2`, `pi * 2^3`, `sqrt(16) + cos(45)`.

---
