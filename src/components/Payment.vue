<template>
  <div>
    <div class="card card-outline-secondary container" v-if="!success">
      <div class="card-body">
        <h3 class="text-center">Credit Card Payment</h3>
        <hr />

        <div class="alert alert-danger p-2 pb-3" v-if="errors.length">
          <a
            class="close font-weight-normal initialism"
            @click="errors.length = 0"
            data-dismiss="alert"
            href="#"
          >
            <samp>×</samp>
          </a>
          <p v-for="error in errors" :key="error+Math.random()">- {{ error }}</p>
        </div>

        <form class="form" role="form" autocomplete="off">
          <div class="form-group">
            <label for="cc_name">Card Holder's Name</label>
            <input
              type="text"
              class="form-control"
              id="cc_name"
              v-model="payment.name"
              title="First and last name"
              required="required"
            />

            <!-- pattern="\w+ \w+.*" -->
          </div>
          <div class="form-group">
            <label>Card Number</label>
            <input
              type="text"
              class="form-control"
              v-model="payment.card_number"
              autocomplete="off"
              maxlength="20"
              pattern="\d{16}"
              title="Credit card number"
              required
            />
          </div>
          <div class="form-group row">
            <label class="col-md-12">Card Exp. Date</label>
            <div class="col-md-4">
              <select class="form-control" v-model="payment.expiry_month" name="cc_exp_mo" size="0">
                <option :key="n" v-for="n in 12">{{ twoDigits(n) }}</option>
              </select>
            </div>
            <div class="col-md-4">
              <select v-model="payment.expiry_year" class="form-control" name="cc_exp_yr" size="0">
                <option>2018</option>
                <option>2019</option>
                <option>2020</option>
                <option>2021</option>
                <option>2022</option>
              </select>
            </div>
            <div class="col-md-4">
              <input
                type="text"
                class="form-control"
                autocomplete="off"
                maxlength="3"
                pattern="\d{3}"
                title="Three digits at back of your card"
                required
                v-model="payment.cvc"
                placeholder="CVC"
              />
            </div>
          </div>
          <div class="row">
            <label class="col-md-12">Amount</label>
          </div>
          <div class="form-inline">
            <div class="input-group">
              <div class="input-group-prepend">
                <span class="input-group-text">$</span>
              </div>
              <input
                type="text"
                class="form-control text-right"
                id="exampleInputAmount"
                :value="plan.price"
                disabled
              />
              <div class="input-group-append">
                <span class="input-group-text">.00</span>
              </div>
            </div>
          </div>
          <hr />
          <div class="form-group row">
            <div class="col-md-6">
              <button
                type="reset"
                @click="$emit('closePayment')"
                class="btn btn-default btn-lg btn-block"
              >Cancel</button>
            </div>
            <div class="col-md-6">
              <button
                @click="makePayment"
                type="submit"
                class="btn btn-success btn-lg btn-block"
              >Submit</button>
            </div>
          </div>
        </form>
      </div>
    </div>
    <Success
      v-if="success"
      @close="success = false; $emit('closePayment')"
      :price="plan.price"
      :name="payment.name"
    />
  </div>
</template>

<script>
import Success from "@/components/Success.vue";
export default {
  components: { Success },
  data() {
    return {
      payment: {},
      errors: [],
      success: false
    };
  },
  props: {
    plan: Object
  },
  methods: {
    twoDigits: n => (String(n).length == 1 ? `0${n}` : n),
    makePayment() {
      const {
        name,
        card_number,
        cvc,
        expiry_year,
        expiry_month
      } = this.payment;
      if (!name || name.length < 6) {
        this.errors = [...this.errors, "name is required"];
      }

      if (
        !card_number ||
        card_number.length < 10 ||
        isNaN(card_number.trim())
      ) {
        this.errors = [
          ...this.errors,
          "card number is required, must be at least 10 numbers"
        ];
      }

      if (!cvc || cvc.length !== 3 || isNaN(cvc.trim())) {
        this.errors = [...this.errors, "cvc is required, must be 3 numbers"];
      }

      if (!expiry_year || !expiry_month) {
        this.errors = [...this.errors, "card expiration date is required"];
      }

      if (this.errors.length) return;
      this.success = true;
    }
  }
};
</script>

<style>
.container {
  width: 100%;
  top: 50px;
}
</style>