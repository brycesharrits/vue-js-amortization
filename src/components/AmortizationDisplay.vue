<template>
  <div class="row">
    <div class="col-8">
      <p>graph and stuff</p>
    </div>
    <div class="col-4">
      <div class="card">
            <h5>Refine Your Results</h5>
            <input v-model="location" placeholder="Atlanta, GA">
            <input v-model="homeValue" placeholder="$300,000">
            <input v-model="interestRate" placeholder="4.1%">
            <input v-model="downPayment" placeholder="$60,000">
            <input v-model="startDate" placeholder="02/10/2022">
            <input v-model="loanTerm" placeholder="30 years">
        </div>
    </div>
  </div>
</template>

<script>
import Vue from 'vue'

export default Vue.extend({
  name: 'AmortizationEntry',
  data: function () {
    return {
      location: null,
      homeValue: null,
      interestRate: null,
      downPayment: null,
      startDate: null,
      loanTerm: null
    }
  }
})
</script>
