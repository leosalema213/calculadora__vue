<script setup>
    import { reactive } from 'vue';

    const estado = reactive({
        previousOperationText: '',
        currentOperationText: '',
        currentOperation: ''
    })

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
    <div class="result">
        <p class="operation"> {{ estado.previousOperationText }}</p>
        <input disabled  type="text" :value="estado.currentOperationText" class="input__result">
    </div>

    <div class="container__buttons"> 
        <ul class="buttons">
            <li class="buttons__row">
                <button @click="click('C')" class="btn btn--destaqued" >C</button>
                <button @click="click('CE')" class="btn btn--destaqued">CE</button>
                <button @click="click('/')" class="btn btn--destaqued lg">
                <img src="../assets/img/icons/icons-divisao.png" alt=""></button>
                <button @click="click('*')" class="btn btn--destaqued">
                    <img src="../assets/img/icons/icons-multiplicacao.png" alt="">
                </button>
            </li>
            <li class="buttons__row">
                <button @click="click('7')" class="btn" >7</button>
                <button @click="click('8')" class="btn" >8</button>
                <button @click="click('9')" class="btn" >9</button>
                <button @click="click('-')" class="btn btn--destaqued">
                    <img src="../assets/img/icons/icons-menos.png" alt="">
                </button>
            </li>
            <li class="buttons__row">
                <button @click="click('4')" class="btn" >4</button>
                <button @click="click('5')" class="btn" >5</button>
                <button @click="click('6')" class="btn" >6</button>
                <button @click="click('+')" class="btn btn--destaqued">
                    <img src="../assets/img/icons/icons-mais.png" alt="">
                </button>
            </li>
            <div class="grid__template">
                <div>
                    <li class="buttons__row">
                        <button @click="click('1')" class="btn" >1</button>
                        <button @click="click('2')" class="btn" >2</button>
                        <button @click="click('3')" class="btn" >3</button>
                    </li>
                    <li class="buttons__row">
                        <button @click="click('0')" class="btn" >0</button>
                        <button @click="click('.')" class="btn" >.</button>
                        <button @click="click('DEL')" class="btn" >
                            <img src="../assets/img/icons/icons-remover.png" alt="">
                        </button>
                    </li>
                </div>
                <li class="buttons__row ">
                    <button @click="click('=')"  class="btn equals__button">
                        <img src="../assets/img/icons/icons-igual.png" alt="">
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
        background-color: rgb(22, 22, 22);
        border-radius: 12px;
        padding: 16px;
    }
    .result .operation {
        font-size: 12px;
        text-align: end;
        color: #8b8b8b;
        margin-bottom: 6px;
    }
    .result input {
        font-size: 24px;
        text-align: end;
        width: 100%;
        background-color: rgb(22, 22, 22);
        border: none;
    }

.grid__template {
    display: grid;
    grid-template-columns: 3fr 1fr;
}
.btn.equals__button {
    margin-left: 2px;
    background-color: rgb(153, 10, 58);
}
.btn.equals__button:hover {
    background-color: rgb(224, 3, 77);
}
    .container__buttons {
        width: 100%;
        height: auto;
        border-radius: 12px;
        padding: 16px;
        margin-top: 40px;
        background-color: rgb(22, 22, 22);
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
        background-color:  rgb(56, 56, 56);
        border: none;
        cursor: pointer;
        font-size: 24px;
    }
    .btn:hover {
        background-color:   rgb(82, 81, 81);
    }
    button.btn--destaqued {
        color: rgb(255, 0, 85);
        background-color:  rgb(82, 81, 81);
    }
    button.btn--destaqued:hover {
        background-color:  rgb(119, 119, 119);
    }
</style>