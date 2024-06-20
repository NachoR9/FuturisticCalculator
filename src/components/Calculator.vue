<script setup>
import { ref } from "vue";

const display = ref("");
const baseCurrency = ref("USD");
const targetCurrency = ref("");

const append = (character) => {
  display.value += character;
};

const clear = () => {
  display.value = "";
  baseCurrency.value = "USD";
  targetCurrency.value = "";
};

const calculate = () => {
  try {
    display.value = eval(display.value).toString();
  } catch (e) {
    display.value = "Error";
  }
};

const setBaseCurrency = (currency) => {
  baseCurrency.value = currency;
};

const convertCurrency = async (currency) => {
  targetCurrency.value = currency;
  try {
    const response = await fetch(
      `https://api.exchangerate-api.com/v4/latest/${baseCurrency.value}`
    );
    const data = await response.json();
    const rate = data.rates[targetCurrency.value];
    display.value = (parseFloat(display.value) * rate).toFixed(2);
  } catch (e) {
    display.value = "Error";
  }
};
</script>

<template>
  <div class="calculator">
    <div class="display">
      {{ display }}
    </div>
    <div class="colFunction">
      <button @click="clear" class="btn-function col-12">CE</button>
    </div>
    <div class="buttons">
      <button @click="append('1')" class="btn-number">1</button>
      <button @click="append('2')" class="btn-number">2</button>
      <button @click="append('3')" class="btn-number">3</button>
      <button @click="append('4')" class="btn-number">4</button>
      <button @click="append('5')" class="btn-number">5</button>
      <button @click="append('6')" class="btn-number">6</button>
      <button @click="append('7')" class="btn-number">7</button>
      <button @click="append('8')" class="btn-number">8</button>
      <button @click="append('9')" class="btn-number">9</button>
      <button @click="append('0')" class="btn-number">0</button>
      <button @click="append('.')" class="btn-number">.</button>
      <button @click="append('*')" class="btn-operator">*</button>
      <button @click="append('+')" class="btn-operator">+</button>
      <button @click="append('/')" class="btn-operator">/</button>
      <button @click="append('-')" class="btn-operator">-</button>
      <button @click="calculate" class="btn-equal">=</button>

      <button @click="setBaseCurrency('USD')" class="btn-currency">USD</button>
      <button @click="setBaseCurrency('EUR')" class="btn-currency">EUR</button>
      <button @click="setBaseCurrency('JPY')" class="btn-currency">JPY</button>
      <button @click="setBaseCurrency('HNL')" class="btn-currency">HNL</button>
      <button @click="convertCurrency('USD')" class="btn-currency">
        to USD
      </button>
      <button @click="convertCurrency('EUR')" class="btn-currency">
        to EUR
      </button>
      <button @click="convertCurrency('JPY')" class="btn-currency">
        to JPY
      </button>
      <button @click="convertCurrency('HNL')" class="btn-currency">
        to HNL
      </button>
    </div>
  </div>
</template>

<style lang="scss" scoped>
.calculator {
  width: 100%;
  position: relative;
  max-width: 420px;
  background: #fff;
  border-radius: 10px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
  overflow: hidden;
  margin: 10px;
  margin-left: 8.5%;
}

.display {
  background: #222;
  color: #fff;
  font-size: 2em;
  padding: 40px;
  text-align: right;
}

.buttons {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 1px;
}

.btn-currency,
.btn-function,
.btn-number,
.btn-operator,
.btn-equal {
  font-size: 1.5em;
  padding: 20px;
  border: none;
  background: #f0f0f0;
  cursor: pointer;
}

button:active {
  background: #ddd;
}

.btn-function {
  background-color: #00000077;
  color: white;
}

.btn-operator {
  background-color: #ff9500;
  color: white;
}

.btn-number {
  background-color: #d4d4d2;
}

.btn-equal {
  background-color: #ff9500;
  color: white;
}

.btn-currency {
  background-color: #007aff;
  color: white;
}
</style>
