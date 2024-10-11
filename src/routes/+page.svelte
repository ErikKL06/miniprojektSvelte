<script>
    import Header from '$lib/components/Header.svelte';
    import Keyboard from '$lib/components/Keyboard.svelte';

 
    let display = "";
    function updateDisplay(e){
        let val = e.target.value; // Knappens value-värde
        if(val === "ac"){
            display = "";
        }else if(val === "c"){
            display = display.slice(0, -1);
        }else if(Number(val) >= 0 && Number(val) <10 ){
            display += val;
        }
    }
</script>

<fieldset>
    <legend>KeyBoard</legend>
    <input type="text" value={display} disabled />
    <Keyboard on:click = {updateDisplay}/>
 </fieldset>
 
    
<Header />
 



