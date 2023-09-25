<script setup>
import { computed, ref } from 'vue'

const tips = ref([
    {
      id: 0,
      value: 5,
      isSelected: false
    },
    {
      id: 1,
      value: 10,
      isSelected: false
    },
    {
      id: 2,
      value: 15,
      isSelected: false
    },
    {
      id: 3,
      value: 20,
      isSelected: false
    },
    {
      id: 4,
      value: 50,
      isSelected: false
    }
  ]
)

const settings = ref({
  bill: null,
  tip: null,
  people: null
})

const totalAmomunt = computed(() => {
  if(settings.value.people > 0){
    let total = (settings.value.bill + (settings.value.bill * (settings.value.tip / 100))) / settings.value.people
    return total.toFixed(2)
  }
  else return parseFloat(0).toFixed(2)
})

const tipAmount = computed(() => {
  if(settings.value.people > 0){
        let tipPerPerson = (settings.value.bill * (settings.value.tip / 100))/ settings.value.people
        return tipPerPerson.toFixed(2)
      }
      else return parseFloat(0).toFixed(2)
})

function resetSettings() {
  settings.value = {
    bill: null,
    tip: null,
    people: null
  }
  tips.value = tips.value.map(tip => ({...tip, isSelected: false}))
}

function resetTips(){
  tips.value = tips.value.map(tip => ({...tip, isSelected: false}))
} 

function handleTipSelect(id, value){
    tips.value = tips.value.map(tip => tip.id === id ? {...tip, isSelected: true} : {...tip, isSelected: false})
    settings.value.tip = value 
}
</script>

<template>
  <div>
    <img src='/images/logo.svg' class='logo' alt='Logo' />
      <main>
        <section class='settings-section'>
          <div class='bill-container'>
            <p class='bill-text'>Bill</p>
            <div class='input-container'>
              <img class='icon dollar' src='/images/icon-dollar.svg' alt='Dollar Icon' />
              <input 
                type='number'
                min={0}
                class='bill-input' 
                name='bill' 
                placeholder='0'
                v-model="settings.bill"  
              />
            </div>
          </div>
          <div class='tip-container'>
            <p class='tip-text'>Select Tip %</p>
            <div class='tip-select-container'>
              <button 
                v-for="tip in tips"
                class="tip-selector tip-button"
                :class="tip.isSelected ? 'selected' : ''"
                :key="tip.id"
                name='tip' 
                @click="handleTipSelect(tip.id, tip.value)"
              >
                {{tip.value}}%
              </button>
              <input 
                type='number' 
                min={0}
                pattern='[0-9]'
                placeholder='Custom' 
                class='tip-selector tip-input' 
                @click="resetTips"
                name='tip'
                v-model="settings.tip"
              />
            </div>
          </div>
          <div class='people-container'>
            <div class='people-text-container'>
              <p class='people-text'>Number of People</p>
              <p class='error'>{{settings.people == 0 ? "Can't be zero" : "" }}</p>
            </div>
           
            <div class='input-container'>
              <img class='icon person' src='/images/icon-person.svg' alt='People Icon' />
              <input 
                type='number'
                name="people"
                min={0}
                class="people-input"
                :class="settings.people == 0 ? 'input-error' : '' "
                placeholder='0'
                v-model="settings.people"
              />
            </div>
          </div>
        </section>
        <section class='results-section'>
          <div class='amount-container'>
            <div class='amount-text'>
              <p class='amount-text-bold'>Tip amount</p>
              <p class='amount-text-normal'>/person</p>
            </div>
            <h1 class='amount-result'>{{ tipAmount }}</h1>
          </div>
          <div class='total-container'>
            <div class='total-text'>
              <p class='total-text-bold'>Total</p>
              <p class='total-text-normal'>/person</p>
            </div>
            <h1 class='total-result'>{{totalAmomunt}}</h1>
          </div>
          <button 
           class='reset-btn' 
           :disabled="totalAmomunt === 0"
           @click="resetSettings"
          >Reset</button>
        </section>
      </main>
      <footer class="attribution">
        Challenge by <a href="https://www.frontendmentor.io?ref=challenge" target="_blank">Frontend Mentor</a>. 
        Coded by <a href="https://github.com/aveandrian">aveandrian</a>.
      </footer>
  </div>
</template>

<style scoped>
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    font-family: 'Space Mono', monospace;
}

body, #root {
    min-height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
    background-color: hsl(185, 41%, 84%);
}

.logo {
    position: absolute;
    top: 10%;
}


main {
    width: 700px;
    padding: 1.25rem;
    border-radius: 0.938rem;
    background-color: white;
    display: flex;
    gap: 1.25rem
}

.settings-section{
    display: flex;
    flex-direction : column;
    gap: 1.875rem;
    width: 50%;
    padding: 0 1.25rem;
}

input {
    border: none;
    font-size: 1.5rem;
    text-align: end;
    width: 100%;
    padding: 0 0.625rem;
    border-radius: 0.313rem;
    background-color: hsl(189, 41%, 97%);
    color:hsl(183, 100%, 15%);
    border: solid 0.125rem transparent
}

input:focus {
    border: solid 0.125rem hsl(172, 67%, 45%)
}

input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button {
  -webkit-appearance: none;
  margin: 0;
}

/* Firefox */
input[type=number] {
  -moz-appearance: textfield;
}

input::placeholder {
    font-size: 1.5rem;
    text-align: right;
    color:  hsl(186, 14%, 43%);
}

.tip-input::placeholder {
    font-size: 1.125rem;
}

input:focus {
    outline: none;
}

.input-container {
    position: relative;
}

.icon {
    position: absolute;
    top: 25%;
    left: 0.625rem;
}

.tip-select-container {
    width: 100%;
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 0.625rem;
}

.tip-selector {
    width: 90px;
    height: 40px;
    font-size: 1.25rem;
    border: none;
    border-radius: 0.313rem;
    color: white;
    background-color: hsl(183, 100%, 15%);
}

.tip-button:hover{
    background-color: rgb(159, 232, 223);
    color: hsl(183, 100%, 15%)
}

.tip-button.selected{
    background-color: hsl(172, 67%, 45%);
}

.tip-input {
    background-color: hsl(189, 41%, 97%);
    color:hsl(183, 100%, 15%);
}

.bill-text, .tip-text, .people-text {
    margin-bottom: 0.313rem;
    color:hsl(184, 14%, 56%);
    font-size: 1rem;
}

input:hover, button:hover {
    cursor: pointer;
}
.people-text-container {
    display: flex;
    justify-content: space-between;
    align-items: baseline;
}
.error {
    color: red;
    font-size: 0.625rem
}
.input-error {
    border: solid 0.125rem red;
}

.results-section {
    width: 50%;
    padding: 2.5rem 1.25rem  1.25rem;
    border-radius: 0.938rem;
    display: flex;
    flex-direction: column;
    gap: 1.563rem;
    background-color: hsl(183, 100%, 15%);
}

.amount-container, .total-container {
    display:flex;
    justify-content: space-between;
    align-items: center;
}

.amount-text-bold, .total-text-bold {
    font-size: 0.875rem;
    font-weight: bold;
    color: white;
}

.amount-text-normal, .total-text-normal {
    font-size: 0.75rem;
    color: hsl(186, 14%, 43%);
}

.amount-result, .total-result {
    font-size: 2rem;
    color: hsl(172, 67%, 45%);
}

.reset-btn {
    margin-top: auto;
    height: 40px;
    border-radius: 0.313rem;
    border: none;
    text-transform: uppercase;
    background-color: hsl(172, 67%, 45%);
    color: hsl(183, 100%, 15%);
}

.reset-btn:disabled {
    background-color: rgb(13, 104, 109);
}

.reset-btn:disabled:hover {
    cursor: auto;
}


footer, footer > a {
    text-align: center;
    margin-top: 1.25rem;
}

@media screen and (max-width: 700px) {
    #root {
        padding: 1.25rem 0;
    }
    .logo {
        position: relative;
        margin-bottom: 1.25rem;
    }
    main {
        flex-direction: column;
        width: 100%;
        align-items: center;
    }

    .settings-section, .results-section {
        width: 100%;
    }

    .tip-select-container {
        grid-template-columns: repeat(2, 1fr);
    }

    .tip-selector {
        width: 100%;
    }
    
}
</style>
