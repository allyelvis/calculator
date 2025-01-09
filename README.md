Sure! Below is an example of a simple calculator using HTML, CSS, and JavaScript.

### HTML (index.html)
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Simple Calculator</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <div class="calculator">
        <input type="text" class="calculator-screen" value="" disabled />
        <div class="calculator-keys">
            <button type="button" class="operator" value="+">+</button>
            <button type="button" class="operator" value="-">-</button>
            <button type="button" class="operator" value="*">&times;</button>
            <button type="button" class="operator" value="/">&divide;</button>
            <button type="button" value="7">7</button>
            <button type="button" value="8">8</button>
            <button type="button" value="9">9</button>
            <button type="button" value="4">4</button>
            <button type="button" value="5">5</button>
            <button type="button" value="6">6</button>
            <button type="button" value="1">1</button>
            <button type="button" value="2">2</button>
            <button type="button" value="3">3</button>
            <button type="button" value="0">0</button>
            <button type="button" class="decimal" value=".">.</button>
            <button type="button" class="all-clear" value="all-clear">AC</button>
            <button type="button" class="equal-sign" value="=">=</button>
        </div>
    </div>
    <script src="script.js"></script>
</body>
</html>
```

### CSS (styles.css)
```css
body {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
    background-color: #f0f0f0;
    margin: 0;
    font-family: 'Arial', sans-serif;
}

.calculator {
    border-radius: 5px;
    box-shadow: 0px 0px 15px -5px rgba(0, 0, 0, 0.75);
    overflow: hidden;
}

.calculator-screen {
    width: 100%;
    height: 80px;
    border: none;
    background-color: #252525;
    color: white;
    text-align: right;
    padding-right: 20px;
    padding-left: 10px;
    font-size: 2.5rem;
}

.calculator-keys {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
}

button {
    height: 60px;
    border: 1px solid #e5e5e5;
    background-color: white;
    font-size: 1.5rem;
}

button.operator {
    background-color: #f2a33c;
    color: white;
}

button.equal-sign {
    background-color: #4caf50;
    color: white;
    grid-column: span 4;
}

button.all-clear {
    background-color: #f44336;
    color: white;
    grid-column: span 2;
}

button.decimal {
    grid-column: span 2;
}
```

### JavaScript (script.js)
```javascript
const calculator = {
    displayValue: '0',
    firstOperand: null,
    waitingForSecondOperand: false,
    operator: null,
};

function inputDigit(digit) {
    const { displayValue, waitingForSecondOperand } = calculator;

    if (waitingForSecondOperand === true) {
        calculator.displayValue = digit;
        calculator.waitingForSecondOperand = false;
    } else {
        calculator.displayValue = displayValue === '0' ? digit : displayValue + digit;
    }
}

function inputDecimal(dot) {
    if (calculator.waitingForSecondOperand === true) return;

    if (!calculator.displayValue.includes(dot)) {
        calculator.displayValue += dot;
    }
}

function handleOperator(nextOperator) {
    const { firstOperand, displayValue, operator } = calculator;
    const inputValue = parseFloat(displayValue);

    if (operator && calculator.waitingForSecondOperand) {
        calculator.operator = nextOperator;
        return;
    }

    if (firstOperand == null) {
        calculator.firstOperand = inputValue;
    } else if (operator) {
        const currentValue = firstOperand || 0;
        const result = performCalculation[operator](currentValue, inputValue);

        calculator.displayValue = String(result);
        calculator.firstOperand = result;
    }

    calculator.waitingForSecondOperand = true;
    calculator.operator = nextOperator;
}

const performCalculation = {
    '/': (firstOperand, secondOperand) => firstOperand / secondOperand,
    '*': (firstOperand, secondOperand) => firstOperand * secondOperand,
    '+': (firstOperand, secondOperand) => firstOperand + secondOperand,
    '-': (firstOperand, secondOperand) => firstOperand - secondOperand,
    '=': (firstOperand, secondOperand) => secondOperand
};

function resetCalculator() {
    calculator.displayValue = '0';
    calculator.firstOperand = null;
    calculator.waitingForSecondOperand = false;
    calculator.operator = null;
}

function updateDisplay() {
    const display = document.querySelector('.calculator-screen');
    display.value = calculator.displayValue;
}

updateDisplay();

const keys = document.querySelector('.calculator-keys');
keys.addEventListener('click', (event) => {
    const { target } = event;
    if (!target.matches('button')) {
        return;
    }

    if (target.classList.contains('operator')) {
        handleOperator(target.value);
        updateDisplay();
        return;
    }

    if (target.classList.contains('decimal')) {
        inputDecimal(target.value);
        updateDisplay();
        return;
    }

    if (target.classList.contains('all-clear')) {
        resetCalculator();
        updateDisplay();
        return;
    }

    inputDigit(target.value);
    updateDisplay();
});
```

### Explanation:
1. **HTML**: This defines the structure of the calculator. It includes an input field to display the result and buttons for digits, operators, decimal point, and an all-clear button.
2. **CSS**: This styles the calculator. It ensures the layout is clean and visually appealing.
3. **JavaScript**: This handles the calculator's functionality. It manages the input, performs calculations, and updates the display.

You can copy these files into your project directory and open the `index.html` file in a web browser to see the calculator in action.
