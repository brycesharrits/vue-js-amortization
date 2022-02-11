<template>
  <div class="card">
    <div class="row">
      <div class="col-8">
        <div class="card p-3">
          <p>Monthly principal and interest</p>
          <h2>{{monthlyPayment}}</h2>

          <table>
            <tr>
              <th>Payment #</th>
              <th>Principal</th>
              <th>Interest</th>
              <th>Balance</th>
            </tr>
            <tr v-for="payment in payments" :key="payment.number">
              <td>{{payment.number}}</td>
              <td>{{payment.principalPayment}}</td>
              <td>{{payment.interestPayment}}</td>
              <td>{{payment.stillOwe}}</td>
            </tr>
          </table>
years
           <table>
            <tr>
              <th>Year</th>
              <th>Principal</th>
              <th>Interest</th>
              <th>Balance</th>
            </tr>
            <tr v-for="year in years" :key="year.number">
              <td>{{year.number}}</td>
              <td>{{year.principalPayment}}</td>
              <td>{{year.interestPayment}}</td>
              <td>{{year.stillOwe}}</td>
            </tr>
          </table>
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

          <button class="btn-primary" @click="calc"> Calculate </button>
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
      homeValue: null, // user entry
      interestRate: null, // user entry
      downPayment: 0, // user entry
      loanTerm: 30, // user entry
      payments: [],
      years: [],
      principal: null,
      numPayments: null,
      r: null,
      monthlyPayment: null
      // loading state?
    }
  },
  methods: {
    calc () {
      if (this.homeValue && this.interestRate && this.loanTerm) {
        this.principal = (this.downPayment) ? this.homeValue - this.downPayment : this.homeValue
        this.numPayments = 12 * parseInt(this.loanTerm)
        console.log(this.interestRate)
        this.r = parseFloat(this.interestRate) / 100 / 12
        console.log(this.r)
        this.monthlyPayment = Math.round((this.r / (1 - ((1 + this.r) ** (this.numPayments * -1)))) * (parseInt(this.homeValue) - parseInt(this.downPayment)))
        this.calcPayments()
      } else {
        this.monthlyPayment = null
        // null out everything?
      }
    },
    calcPayments () {
      this.payments = []
      let remainingBalance = this.principal

      for (let index = 0; index < this.numPayments; index++) {
        const payment = {}
        payment.number = index
        payment.stillOwe = Math.round(remainingBalance)
        payment.interestPayment = (index !== 0) ? Math.round(remainingBalance * (this.r)) : 0
        payment.principalPayment = (index !== 0) ? Math.round(this.monthlyPayment - payment.interestPayment) : 0
        this.payments.push(payment)
        remainingBalance = remainingBalance - payment.principalPayment
      }
      this.calcYears()
    },
    calcYears () {
      this.years = []

      this.payments.forEach((payment) => {
        const yearNumber = Math.floor(payment.number / 12)
        let year = {}
        year.number = yearNumber
        year.interestPayment = 0
        year.principalPayment = 0
        if (this.years.filter((year) => year.number === yearNumber).length) {
          year = this.years.filter((year) => year.number === yearNumber)[0]
          this.years = this.years.filter((year) => year.number !== yearNumber)
        }

        year.interestPayment = parseInt(year.interestPayment) + parseInt(payment.interestPayment)
        year.principalPayment = parseInt(year.principalPayment) + parseInt(payment.principalPayment)
        this.years.push(year)
      })

      let stillOwed = this.principal
      this.years.forEach((year) => {
        year.stillOwe = stillOwed
        stillOwed = stillOwed - year.principalPayment
      })
    }
  }
})
</script>
