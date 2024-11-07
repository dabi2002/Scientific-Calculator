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

```java
private ArrayList<String> toPostfix(String expression) {
    Stack<String> operators = new Stack<>();
    ArrayList<String> output = new ArrayList<>();
    StringTokenizer tokenizer = new StringTokenizer(expression, "+-*/^() ", true);

    while (tokenizer.hasMoreTokens()) {
        String token = tokenizer.nextToken().trim();
        if (token.isEmpty()) continue;

        if (isNumber(token)) {
            output.add(token);
        } else if (isOperator(token)) {
            while (!operators.isEmpty() && getPrecedence(operators.peek()) >= getPrecedence(token)) {
                output.add(operators.pop());
            }
            operators.push(token);
        } else if (token.equals("(")) {
            operators.push(token);
        } else if (token.equals(")")) {
            while (!operators.isEmpty() && !operators.peek().equals("(")) {
                output.add(operators.pop());
            }
            operators.pop(); // Remover '('
        } else if (isFunction(token)) {
            operators.push(token);
        }
    }

    // Vaciar operadores restantes
    while (!operators.isEmpty()) {
        output.add(operators.pop());
    }
    return output;
}
```

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

---

## 3. Evaluación de Expresión Postfija (`evaluatePostfix`)

Una vez que la expresión está en notación postfija, `evaluatePostfix` evalúa la expresión utilizando una pila (`stack`).

```java
private double evaluatePostfix(ArrayList<String> tokens) {
    Stack<Double> stack = new Stack<>();

    for (String token : tokens) {
        if (isNumber(token)) {
            stack.push(Double.parseDouble(token));
        } else if (isFunction(token)) {
            if (token.equals("pi")) {
                stack.push(Math.PI);
            } else {
                double value = stack.pop();
                switch (token) {
                    case "sin":
                        stack.push(Math.sin(Math.toRadians(value)));
                        break;
                    case "cos":
                        stack.push(Math.cos(Math.toRadians(value)));
                        break;
                    case "tan":
                        stack.push(Math.tan(Math.toRadians(value)));
                        break;
                    case "sqrt":
                        stack.push(Math.sqrt(value));
                        break;
                    case "factorial":
                        stack.push((double) factorial((int) value));
                        break;
                }
            }
        } else if (isOperator(token)) {
            double b = stack.pop();
            double a = stack.pop();
            switch (token) {
                case "+":
                    stack.push(a + b);
                    break;
                case "-":
                    stack.push(a - b);
                    break;
                case "*":
                    stack.push(a * b);
                    break;
                case "/":
                    stack.push(a / b);
                    break;
                case "^":
                    stack.push(Math.pow(a, b));
                    break;
            }
        }
    }

    return stack.pop();
}
```

### Explicación
1. **Operadores y Números:**  
   Los números se apilan, mientras que los operadores toman dos valores de la pila y aplican la operación.

2. **Funciones Científicas:**  
   Las funciones toman un valor de la pila. Ejemplo: `sin` convierte grados a radianes y luego aplica `Math.sin`.

3. **Constantes:**  
   `pi` se añade a la pila directamente como `Math.PI`.

---

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
