<template>
  <div class="home">
    <h2>Mortgage Amortization Calculator</h2>
    <p>Your amortization schedule will show you how much of your monthly mortgage payments you spend toward principal and interest.</p>
    <img alt="Vue logo" src="../assets/logo.png">
    <HelloWorld msg="Welcome to Your Vue.js + TypeScript App"/>
    <AmortizationDisplay msg="Welcome to Your Vue.js + TypeScript App"/>
    <AmortizationEntry msg="Welcome to Your Vue.js + TypeScript App"/>
  </div>
</template>

<script>
import Vue from 'vue'
import AmortizationDisplay from '@/components/AmortizationDisplay.vue' // @ is an alias to /src
import AmortizationEntry from '@/components/AmortizationEntry.vue' // @ is an alias to /src

export default Vue.extend({
  name: 'Amortization',
  components: {
    AmortizationDisplay,
    AmortizationEntry
  }
})
</script>
