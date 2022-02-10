<template>
  <div class="card">
    <div class="row">
      <div class="col-8">
        <div class="card p-3">
          <p>Monthly principal and interest</p>
          <h2>{{monthly}}</h2>
        </div>
      </div>
      <div class="col-4">
        <div class="card p-3">
          <h5>Refine Your Results</h5>

          <label class="form-label mb-0" style="text-align:left">Home value</label>
          <input v-model="homeValue" class="form-control mb-2" placeholder="$300,000"> <!--v-mask?-->

          <label class="form-label mb-0" style="text-align:left">Interest rate</label>
          <input v-model="interestRate" class="form-control mb-2" placeholder="4.1%"> <!--v-mask?-->

          <label class="form-label mb-0" style="text-align:left">Down payment</label>
          <input v-model="downPayment" class="form-control mb-2" placeholder="$60,000"> <!--v-mask?-->

          <label class="form-label mb-0" style="text-align:left">Loan term (years)</label>
          <input v-model="loanTerm" class="form-control mb-2" placeholder="30"> <!--v-mask?-->

        </div>
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
      homeValue: null,
      interestRate: null,
      downPayment: 0,
      loanTerm: 30
    }
  },
  computed: {
    monthly () { // might need to convert interest rate to decimal?
      let numPayments = 0
      if (this.homeValue && this.interestRate && this.loanTerm) {
        numPayments = 12 * parseInt(this.loanTerm)
        console.log(this.interestRate)
        const r = parseFloat(this.interestRate) / 100 / 12
        console.log(r)
        return (r / (1 - ((1 + r) ** (numPayments * -1)))) * (parseInt(this.homeValue) - parseInt(this.downPayment)).toFixed(2)
      } else {
        return 'Need more info'
      }
    }
  }
})
</script>
