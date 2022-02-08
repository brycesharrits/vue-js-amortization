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

          <label class="form-label mb-0" style="text-align:left">Location</label>
          <input v-model="location" class="form-control mb-2" placeholder="Atlanta, GA"> <!--Need some kind of service to pull locations-->

          <label class="form-label mb-0" style="text-align:left">Home value</label>
          <input v-model="homeValue" class="form-control mb-2" placeholder="$300,000"> <!--v-mask?-->

          <label class="form-label mb-0" style="text-align:left">Interest rate</label>
          <input v-model="interestRate" class="form-control mb-2" placeholder="4.1%"> <!--v-mask?-->

          <label class="form-label mb-0" style="text-align:left">Down payment</label>
          <input v-model="downPayment" class="form-control mb-2" placeholder="$60,000"> <!--v-mask?-->

          <label class="form-label mb-0" style="text-align:left">Start date</label>
          <input v-model="startDate" class="form-control mb-2" placeholder="02/10/2022"> <!--v-mask? or date picker-->

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
      location: null,
      homeValue: null,
      interestRate: null,
      downPayment: null,
      startDate: null,
      loanTerm: 30
    }
  },
  computed: {
    monthly () { // might need to convert interest rate to decimal?
      let numPayments = 0
      if (this.loanTerm) {
        numPayments = 12 * this.loanTerm
      }
      if (this.interestRate) {
        return (this.interestRate / (1 - ((1 + this.interestRate) ** (numPayments * -1)))) * (this.homeValue - this.downPayment)
      }
      return 'Need more info'
    }
  }
})
</script>
