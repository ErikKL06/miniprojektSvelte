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
    let display = " ";
    
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
            display += val;
            if (check === true) {
            calculate();
            check = false;
            }
            memory = lcd.value;
            isCalculated = false;

        }else if(val === "enter"){
            calculate();
            check = false;

        }else if(val === "."){
            display += val;
            isCalculated = false;
            isComma = true;
        }
    }

    function setOperator(operator){
        arithmetic = operator;
    }

    function calculate(){

        if (arithmetic === "+") {
            if (lastOperand === null) {
                    lastOperand = parseFloat(display);
                }
            display = parseFloat(memory) + lastOperand;
            memory = display;
            lastOperation = '+';
        }
        else if (arithmetic === "-") {
            if (lastOperand === null) {
                lastOperand = parseFloat(display);
            }
            display = parseFloat(memory) - lastOperand;
            memory = display;
            lastOperation = '-';
        }
        else if (arithmetic === "/") {
            if (lastOperand === null) {
                lastOperand = parseFloat(display);
            }
            display = parseFloat(memory) / lastOperand;
            memory = display;
            lastOperation = '/';
        }
        else {
            if (lastOperand === null) {
                lastOperand = parseFloat(display);
            }
            display = parseFloat(memory) * lastOperand;
            memory = display;
            lastOperation = '*';
        }
        arithmetic = lastOperation;

        isCalculated = true;

    }
    

    /** Rensar display */
    function clearLCD() {
        display = '';
        isComma = false;
        lastOperand = null;
        lastOperation = null;
        console.log("clear");
        if(display === '') {
            document.getElementById("clear").innerHTML = "MEMCLEAR";
        }

    }

    function memClear(){
        check = false;
        memory = 0;
        arithmetic = null;
        clearLCD();
        console.log("memClear");
    }

</script>

<fieldset>
    <legend>KeyBoard</legend>
    <input type="text" value={display} disabled />
    <Keyboard on:click = {updateDisplay}/>
 </fieldset>
 
    
<Header />
 



