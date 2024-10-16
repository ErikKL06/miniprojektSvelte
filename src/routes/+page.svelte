<script>
    import Header from '$lib/components/Header.svelte';
    import Keyboard from '$lib/components/Keyboard.svelte';

    let isComma = false;
    let lastOperand = null;
    let lastOperation = null;
    let check = false;
    let isCalculated = false;
    let memory = 0; // Lagrat/gamlat värdet från display
    let arithmetic = null; // Vilken beräkning som skall göras +,-, x eller /
    let display = '';
    
    function updateDisplay(e){
        let val = e.target.value; // Knappens value-värde
        if(val === "ac"){
            memClear();

        }else if(val === "c"){
            display = display.slice(0, -1);

        }else if(Number(val) >= 0 && Number(val) <10 ){

            if (isCalculated === true) {
                memClear();
            }
            else if (arithmetic !== null && isComma === false) {
                clearLCD();
                check = true;
            }
            display += val;
            isComma = false;
            isCalculated = false;

        }else if(val === "+" || val === "-" || val === "*" || val === "/"){
            if (check === true) {
                calculate();
                check = false;
            }
            memory = parseFloat(display); // Convert display to number
            setOperator(val);
            isCalculated = false;
            display = "";
        }else if(val === "enter"){
            calculate();
            check = false;
        }else if(val === "."){
            if (!isComma) {
                display += val;
                isComma = true;
            }
        }
    }

    function setOperator(operator) {
        arithmetic = operator;
    }

    function calculate() {
        let currentOperand = parseFloat(display); // Convert display to number
        if (arithmetic === "+") {
            display = (memory + currentOperand).toString();
        } else if (arithmetic === "-") {
            display = (memory - currentOperand).toString();
        } else if (arithmetic === "*") {
            display = (memory * currentOperand).toString();
        } else if (arithmetic === "/") {
            display = (memory / currentOperand).toString();
        }
        isCalculated = true;
        isComma = false;
        lastOperand = null;
        arithmetic = null;
    }

    function memClear() {
        display = "";
        memory = 0;
        arithmetic = null;
        isComma = false;
        isCalculated = false;
        lastOperand = null;
        lastOperation = null;
        check = false;
    }

    function clearLCD() {
        display = "";
    }
</script>

<fieldset>
    <legend>KeyBoard</legend>
    <input type="text" bind:value={display} readonly />
    <Keyboard on:click={updateDisplay} />
</fieldset>
