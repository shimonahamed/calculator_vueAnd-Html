<template>
    <div class="calculator">
        <input class="output form-control" v-model="display" readonly />
        <div class="buttons">
            <button @click="appendToDisplay('7')" class="btn btn-light">7</button>
            <button @click="appendToDisplay('8')" class="btn btn-light">8</button>
            <button @click="appendToDisplay('9')" class="btn btn-light">9</button>
            <button @click="appendToDisplay('/')" class="btn btn-warning">/</button>
            <button @click="appendToDisplay('4')" class="btn btn-light">4</button>
            <button @click="appendToDisplay('5')" class="btn btn-light">5</button>
            <button @click="appendToDisplay('6')" class="btn btn-light">6</button>
            <button @click="appendToDisplay('*')" class="btn btn-warning">*</button>

            <button @click="appendToDisplay('1')" class="btn btn-light">1</button>
            <button @click="appendToDisplay('2')" class="btn btn-light">2</button>
            <button @click="appendToDisplay('3')" class="btn btn-light">3</button>
            <button @click="appendToDisplay('-')" class="btn btn-warning">-</button>

            <button @click="appendToDisplay('0')" class="btn btn-light">0</button>
            <button @click="appendToDisplay('.')" class="btn btn-light">.</button>
            <button @click="calculateResult()" class="btn btn-success">=</button>
            <button @click="appendToDisplay('+')" class="btn btn-warning">+</button>
        </div>
        <button @click="clearDisplay()" class="btn btn-danger w-50 mt-3">AC</button>
        <button @click="deleteLastCharacter()" class="btn btn-danger w-50 mt-3"><i class="fa fa-close"></i></button>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                display: ''
            };
        },
        methods: {
            appendToDisplay(value) {
                const operators = ['+', '-', '*', '/'];
                if (operators.includes(value) && operators.includes(this.display.slice(-1))) {
                    return;
                }
                this.display += value;
            },
            calculateResult() {
                try {
                    this.display = eval(this.display).toString();
                } catch (e) {
                    this.display = 'Error';
                }
            },
            clearDisplay() {
                this.display = '';
            },
            deleteLastCharacter() {
                this.display = this.display.slice(0, -1);
            },
            handleKeyboardInput(event) {
                const key = event.key;
                const operators = ['+', '-', '*', '/', '.'];
                if (operators.includes(key) && operators.includes(this.display.slice(-1))) {
                    return;
                }
                if (['0', '1', '2', '3', '4', '5', '6', '7', '8', '9', '+', '-', '*', '/', '.', 'Enter'].includes(key)) {
                    if (key === 'Enter') {
                        this.calculateResult();
                    } else {
                        this.appendToDisplay(key);
                    }
                } else if (key === 'Backspace') {
                    this.deleteLastCharacter();
                }
            }
        },
        mounted() {
            window.addEventListener('keydown', this.handleKeyboardInput);
        },
        beforeDestroy() {
            window.removeEventListener('keydown', this.handleKeyboardInput);
        }
    };
</script>

<style scoped>
    .calculator {
        width: 250px;
        margin: 100px auto;
        padding: 20px;
        border: 1px solid #ccc;
        border-radius: 5px;
        background-color: #f8f9fa;
    }

    .buttons {
        display: grid;
        grid-template-columns: repeat(4, 1fr);
        gap: 10px;
    }

    .buttons button {
        padding: 20px;
        font-size: 18px;
        cursor: pointer;
        border: 1px solid #ccc;
        border-radius: 5px;
        transition: all 0.3s ease;
    }

    .buttons button:hover {
        background-color: #e0e0e0;
    }

    .output {
        width: 100%;
        padding: 15px;
        font-size: 24px;
        text-align: right;
        margin-bottom: 20px;
        border-radius: 5px;
    }

    .btn-light {
        background-color: #f1f1f1;
    }

    .btn-warning {
        background-color: #ffc107;
        color: white;
    }

    .btn-success {
        background-color: #28a745;
        color: white;
    }

    .btn-danger {
        background-color: #dc3545;
        color: white;
    }
</style>
