<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>calculator</title>
   
   
    <style>
    body {
        display: flex;
        justify-content: center;
        align-items: center;
        background-color: #f4f4f9;
        font-family: 'Arial', sans-serif;
    }
    
    .calculator-keys {
        display: grid;
        grid-template-columns: repeat(4, 1fr);
        
    }
    
    button {
        height: 50px;
        font-size: 1.2rem;
        border-radius: 5px;
        border: none;
        cursor: pointer;
    }
    
    button.operator {
        background-color: #f39c12;
        color: white;
    }
    
    button.equal-sign {
        background-color: #27ae60;
        color: white;
        grid-column: span 2;
    }
    
    button.all-clear {
        background-color: #e74c3c;
        color: white;
        grid-column: span 2;
    }
    
    button:not(.operator):not(.equal-sign):not(.all-clear) {
        background-color: #e0e0e0;
    }
</style>
</head>
<body>
</body>
</html>
<body>

    <div class="calculator">
       calculator <input type="text" class="calculator-screen" id="screen" disabled>
       
        <div class="calculator-keys">
            <button type="button" class="operator" value="+" name="button">+</button>
            <button type="button" class="operator" value="-">-</button>
            <button type="button" class="operator" value="*">&times;</button>
            <button type="button" class="operator" value="/">&divide;</button>

            <button type="button" value="0">0</button>
            <button type="button" value="1">1</button>
            <button type="button" value="2">2</button>
            <button type="button" value="3">3</button>
            <button type="button" value="4">4</button>
            <button type="button" value="5">5</button>
            <button type="button" value="6">6</button>
            <button type="button" value="7">7</button>
            <button type="button" value="8">8</button>
            <button type="button" value="9">9</button>

            <button type="button" class="decimal" value=".">.</button>
            <button type="button" class="all-clear" value="all-clear">AC</button>
            <button type="button" class="equal-sign" value="=">=</button>
        </div>
    </div>
    <script src="script.js">
    // Object to keep track of calculator state
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

    if (firstOperand == null && !isNaN(inputValue)) {
        calculator.firstOperand = inputValue;
    } else if (operator) {
        const result = performCalculation[operator](firstOperand, inputValue);

        calculator.displayValue = `${parseFloat(result.toFixed(7))}`;
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
    '=': (firstOperand, secondOperand) => secondOperand,
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

</script>
</body>

</html>

