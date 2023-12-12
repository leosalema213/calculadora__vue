<!-- eslint-disable vue/multi-word-component-names -->
<script setup>
    import { reactive } from 'vue';

    const estado = reactive({
        previousOperationText: '',
        currentOperationText: '',
        currentOperation: ''
    })

    const props = defineProps(['theme'])

    // Add digit to calculator screen
    const addDigit = (digit) => {
        // check if current operation already has a dot
        if(digit === "." && estado.currentOperationText.includes(".")){
            return;
        }

        estado.currentOperation = digit
        updateScreen()
    }

    // Process all calculator operations
    const processOperation = (operation) => {
        // Check if current is empty
        if(estado.currentOperationText === "" && operation !== "C"){
            // Change operation
            if(estado.previousOperationText !== ""){
                changeOperation(operation)
            }
            return
        }

        // Get current and previous value
        let operationValue;
        const previous = +estado.previousOperationText.split(" ")[0]
        const current = +estado.currentOperationText

        switch(operation) {
            case "+":
            operationValue = previous + current
            updateScreen(operationValue, operation, current, previous)
                break
            case "-":
            operationValue = previous - current
            updateScreen(operationValue, operation, current, previous)
                break
            case "/":
            operationValue = previous / current
            updateScreen(operationValue, operation, current, previous)
                break
            case "*":
            operationValue = previous * current
            updateScreen(operationValue, operation, current, previous)
                break
            case "DEL":
                processDelOperator()
                break
            case "CE":
                processClearCurrentOperation()
                break
            case "C":
                processClearOperation()
                break
            case "=":
                processEqualOperation()
                break
                
            default:
                return
        }
    }

    // Change values of the calculator screen
    const updateScreen = (
        operationValue = null,
        operation = null,
        current = null,
        previous = null
        ) => {
        if(operationValue === null) {
            estado.currentOperationText += estado.currentOperation 
        } else {
            //Check if value is zero, if it is just add current value
            if(previous === 0) {
                operationValue = current
            }

            // Add current value to previous
            estado.previousOperationText = `${operationValue} ${operation}`
            estado.currentOperationText = ""
        }
    }

    // Change math operation
    const changeOperation = (operation) => {
        const mathOperations = ["*", "/", "+", "-"]

        if(!mathOperations.includes(operation)) {
            return
        }

        estado.previousOperationText = estado.previousOperationText.slice(0, -1) + operation;
    }

    // Delete the last digit
    const processDelOperator = () => {
        estado.currentOperationText = estado.currentOperationText.slice(0, -1)
    }

    // Clear current operation
    const processClearCurrentOperation = () => {
        estado.currentOperationText = ""
    }

    // Clear all operations
    const processClearOperation = () => { 
        estado.currentOperationText = ""
        estado.previousOperationText = ""
    }


    // Process operation
    const processEqualOperation = () => {
        const operation = estado.previousOperationText.split(" ")[1];
        processOperation(operation)

    }

    const click = (par) => {
        if(+par >= 0 || par === ".") {
            addDigit(par)
        } else {
            processOperation(par)
        }
    }

</script>

<template>
    <div :class="props.theme === 'light' ? 'result' : 'result dark'">
        <p class="operation"> {{ estado.previousOperationText }}</p>
        <input disabled  type="text" :value="estado.currentOperationText" :class="props.theme === 'light' ? '' : 'dark'">
    </div>

    <div :class="props.theme === 'light' ? 'container__buttons' : 'container__buttons dark'"> 
        <ul class="buttons">
            <li class="buttons__row">
                <button @click="click('C')" :class="props.theme === 'light' ? 'btn btn--destaqued': 'btn btn--dark btn--destaqued--dark'" >C</button>
                <button @click="click('CE')" :class="props.theme === 'light' ? 'btn btn--destaqued': 'btn btn--dark btn--destaqued--dark'">CE</button>
                <button @click="click('/')" :class="props.theme === 'light' ? 'btn btn--destaqued': 'btn btn--dark btn--destaqued--dark'">
                    <img v-if="props.theme === 'light'" src="../assets/img/icons/icons8-dividisão-32.png" alt="/">
                    <img v-else src="../assets/img/icons/dark/icons-divisao.png" alt="/">
                </button>
                <button @click="click('*')" :class="props.theme === 'light' ? 'btn btn--destaqued': 'btn btn--dark btn--destaqued--dark'">
                    <img v-if="props.theme === 'light'" src="../assets/img/icons/icons8-multiplicação-32.png " alt="*">
                    <img v-else src="../assets/img/icons/dark/icons-multiplicacao.png" alt="*">
                </button>
                
            </li> 
            <li class="buttons__row">
                <button @click="click('7')" :class="props.theme === 'light' ? 'btn' : 'btn btn--dark'" >7</button>
                <button @click="click('8')" :class="props.theme === 'light' ? 'btn' : 'btn btn--dark'" >8</button>
                <button @click="click('9')" :class="props.theme === 'light' ? 'btn' : 'btn btn--dark'" >9</button>
                <button @click="click('-')" :class="props.theme === 'light' ? 'btn btn--destaqued': 'btn btn--dark btn--destaqued--dark'">
                    <img v-if="props.theme === 'light'" src="../assets/img/icons/icons8-menos-32.png" alt="-">
                    <img v-else src="../assets/img/icons/dark/icons-menos.png" alt="-">
                </button>
            </li>
            <li class="buttons__row">
                <button @click="click('4')" :class="props.theme === 'light' ? 'btn' : 'btn btn--dark'" >4</button>
                <button @click="click('5')" :class="props.theme === 'light' ? 'btn' : 'btn btn--dark'" >5</button>
                <button @click="click('6')" :class="props.theme === 'light' ? 'btn' : 'btn btn--dark'" >6</button>
                <button @click="click('+')" :class="props.theme === 'light' ? 'btn btn--destaqued': 'btn btn--dark btn--destaqued--dark'">
                    <img v-if="props.theme === 'light'" src="../assets/img/icons/icons8-soma-32.png" alt="+">
                    <img v-else src="../assets/img/icons/dark/icons-mais.png" alt="+">
                </button>
            </li>
            <div class="grid__template">
                <div>
                    <li class="buttons__row">
                        <button @click="click('1')" :class="props.theme === 'light' ? 'btn' : 'btn btn--dark'" >1</button>
                        <button @click="click('2')" :class="props.theme === 'light' ? 'btn' : 'btn btn--dark'" >2</button>
                        <button @click="click('3')" :class="props.theme === 'light' ? 'btn' : 'btn btn--dark'" >3</button>
                    </li>
                    <li class="buttons__row">
                        <button @click="click('0')" :class="props.theme === 'light' ? 'btn' : 'btn btn--dark'" >0</button>
                        <button @click="click('.')" :class="props.theme === 'light' ? 'btn' : 'btn btn--dark'" >.</button>
                        <button @click="click('DEL')" :class="props.theme === 'light' ? 'btn' : 'btn btn--dark'" >
                            <img v-if="props.theme === 'light'" src="../assets/img/icons/icons8-backspace-32.png" alt="del">
                            <img v-else src="../assets/img/icons/dark/icons-remover.png  " alt="del">
                        </button>
                    </li>
                </div>
                <li class="buttons__row ">
                    <button @click="click('=')"  :class="props.theme === 'light' ? 'btn equals__button--light' : 'btn equals__button--dark'">
                        <img v-if="props.theme === 'light'" src="../assets/img/icons/icons-igual.png" alt="=">
                        <img v-else src="../assets/img/icons/dark/icons-igual.png" alt="=">
                        
                    </button>
                </li>
            </div>
        </ul>
    </div>
</template>
<style scoped>

    .result {
        width: 100%;
        height: auto;
        background-color: #b4c4f8;
        border-radius: 12px;
        padding: 16px;
    }
    .result.dark,
    .result input.dark,
    .container__buttons.dark{
        background-color: #000000;
        color: #fff;
    }    
    .result .operation {
        font-size: 1em;
        text-align: end;
        color: #696969;
        margin-bottom: 6px;
    }
    .result input {
        font-size: 1.5em;
        text-align: end;
        width: 100%;
        background-color: #b4c4f8;
        border: none;
        color: #000;
    }

    .grid__template {
        display: grid;
        grid-template-columns: 3fr 1fr;
    }
    .btn.equals__button--dark {
        margin-left: 2px;
        background-color: rgb(153, 10, 58);
    }
    .btn.equals__button--dark:hover {
        background-color: rgb(224, 3, 77);
    }

    .btn.equals__button--light {
        margin-left: 2px;
        background-color: rgb(42, 117, 255);
    }
    .btn.equals__button--light:hover {
        background-color: rgb(52, 153, 247);
    }
    .container__buttons {
        width: 100%;
        height: auto;
        border-radius: 12px;
        padding: 16px;
        margin-top: 40px;
        background-color: #b4c4f8;
    }
    .buttons {
        width: 100%;
    }
    .buttons__row {
        list-style: none;
        display: flex;
        column-gap: 2px;
        margin-bottom: 2px;
    }

    .btn {
        display: flex;
        align-items: center;
        justify-content: center;
        padding: 16px;
        width: 100%;
        height: auto;
        background-color: #d6d4d4;
        border: none;
        cursor: pointer;
        font-size: 1.5em;
        color: #000;
    }   

    .btn.btn--dark {
        background-color:  rgb(56, 56, 56);
        color: #fff;
    }
    .btn:hover {
        background-color:   rgb(199, 199, 199);
    }
    .btn--dark:hover {
        background-color:   rgb(150, 148, 148);
    }
    .btn.btn--destaqued {
        color: rgb(42, 117, 255);
        background-color:  rgb(255, 255, 255);
    }
    .btn.btn--destaqued:hover {
        background-color:  rgb(231, 229, 229);
    }
    button.btn--destaqued--dark {
        color: rgb(255, 0, 85);
        background-color:  rgb(82, 81, 81);
    }
    button.btn--destaqued--dark:hover {
        background-color:  rgb(119, 119, 119);
    }
</style>