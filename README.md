# Calculadora Científica 

Esta calculadora científica es capaz de realizar operaciones aritméticas básicas (suma, resta, multiplicación, división, potencia) y funciones científicas (seno, coseno, tangente, raíz cuadrada, factorial), así como manejar constantes como pi. Además, el código convierte expresiones de notación infija a notación postfija usando el algoritmo Shunting Yard para una evaluación precisa de la prioridad de operadores.

## Clases y Métodos Principales

- **toPostfix**:  
  Convierte una expresion en notación infija a notación postfija para facilitar la evaluación posterior.
  
- **evaluatePostfix**:  
  Toma una lista de tokens en notación postfija y evalúa el resultado aplicando operadores y funciones correctamente.

- **isNumber**:  
  Verifica si el token es un número.

- **isOperator**:  
  Comprueba si el token es un operador aritmético.

- **isFunction:**  
  Determina si el token es una función cientifica.

## toPostfix

Este metodo es el encargado de convertir la expresión infija (que es la comun por los humanos) a notación postfija. Esto nos permite que los operadores y las funciones se apliquen en el orden correcto durante la evaluación, todo esto lo hace mediante las siguientes condicionales:

1. Si el valor a evaluar es un numero, se agrega directamente a la salida.
2. Si el valor a evaluar es un operador, se desapilan operadores de mayor o igual precedencia antes de apilar el nuevo operador.
3. Si el valor a evaluar es un parentesis izquierdo (, se apila.
4. Si el valor a evaluar es un parentesis derecho ), se desapilan operadores hasta encontrar el paréntesis izquierdo correspondiente.
5. Si valor a evaluar es una función matemática, se apila para procesarse más adelante.
6. Si el token es una constante (pi, e), se reemplaza por su valor numérico y se agrega a la salida.

Al final del recorrido, se vacía la pila de operadores en la salida.

### Ejemplo de Evaluación

#### Expresión en notación infija:
3 + sin(90) * 2

![image](https://github.com/user-attachments/assets/8ad74cc7-21d6-400f-b9ba-cce54d9830c9)

## 3. evaluatePostfix

Una vez que la expresión está en notación postfija, este metodo evalúa la expresión utilizando una pila. Este método recorre una lista de tokens que representan una expresión postfija y la evalúa utilizando los siguientes pasos:

1. Si el valor a evaluar es un número, se convierte a double y se almacena en la pila.
2. Si el valor a evaluar es una función matemática, se extrae el último valor de la pila, se aplica la función y se almacena el resultado.
3. Si el valor a evaluar es un operador, se extraen los dos valores superiores de la pila, se realiza la operación y se almacena el resultado.
4. Al finalizar el recorrido, la pila contendrá el resultado de la expresión.

### Explicación
1. Operadores y Números:  
   Los números se apilan, mientras que los operadores toman dos valores de la pila y aplican su respectiva operacion.

2. Funciones Científicas:  
   Las funciones toman un valor de la pila.

3. Constantes:  
   `pi` se añade a la pila directamente como `Math.PI`.

### Ejemplo de Evaluación

#### Expresión en notación infija: 
  3 + sin(90) * 2

#### Conversión a notación postfija:
![image](https://github.com/user-attachments/assets/c91e6542-98d8-487d-b3a2-c69a19cf20a2)

#### Resultado
  3 90 sin 2 * +

#### Resultado final:
5.0

---
 ## 4. Manejo de Funciones y Constantes Científicas

Para definir funciones como **sin**, **cos**, **tan**, entre otros, hemos implementado los métodos **isFunction** y **evaluatePostfix**.

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
