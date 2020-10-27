<template>
  <div id="app">
    <BancCardBack v-if="showBack" :cvvNumber="cvvNumber" :showBack="showBack" />

    <BancCard
      v-else
      ref="bancCard"
      :cardNumber="cardNumber"
      :cardHolder="cardHolder"
      :expirationMonth="expirationMonth"
      :expirationYear="expirationYear"
      :cardNumberActive="cardNumberActive"
      :cardHolderActive="cardHolderActive"
      :cardExpirationActive="cardExpirationActive"
      :showBack="showBack"
      @focusToCardNumber="focusToCardNumber"
      @focusToCardHolder="focusToCardHolder"
      @focusToCardMonthExpiration="focusToCardMonthExpiration"
      @focusToCardYearExpiration="focusToCardYearExpiration"
    />
    <div class="form">
      <div class="app__form">
        <div class="form__cardNumber">
          <p>Card Number</p>
          <input
            v-mask="'#### #### #### ####'"
            type="text"
            ref="cardNumber"
            v-model="cardNumber"
            @keyup="cardType"
            @focus="cardNumberFocus"
            @blur="cardNumberActive = false"
          />
        </div>
        <div class="form__cardHolder">
          <p>Card Holder</p>
          <input
            type="text"
            ref="cardHolder"
            v-model="cardHolder"
            @focus="cardHolderFocus"
            @blur="cardHolderActive = false"
          />
        </div>
        <div class="form__lilInput">
          <div class="form__date">
            <p>Expiration Date</p>
            <select
              name="month"
              ref="cardExpirationMonth"
              v-model="expirationMonth"
              @focus="cardExpirationMonthFocus"
              @blur="cardExpirationActive = false"
              v-bind:class="{ active: monthSelectActive }"
            >
              <option value disabled selected>Month</option>
              <option value="01">01</option>
              <option value="02">02</option>
              <option value="03">03</option>
              <option value="04">04</option>
              <option value="05">05</option>
              <option value="06">06</option>
              <option value="07">07</option>
              <option value="08">08</option>
              <option value="09">09</option>
              <option value="10">10</option>
              <option value="11">11</option>
              <option value="12">12</option>
            </select>
            <select
              name="year"
              ref="cardExpirationYear"
              v-model="expirationYear"
              @focus="cardExpirationYearFocus"
              @blur="cardExpirationActive = false"
              v-bind:class="{ active: yearSelectActive }"
            >
              <option value disabled selected>Year</option>
              <option value="20">2020</option>
              <option value="21">2021</option>
              <option value="22">2022</option>
              <option value="23">2023</option>
              <option value="24">2024</option>
              <option value="25">2025</option>
              <option value="26">2026</option>
              <option value="27">2027</option>
              <option value="28">2028</option>
              <option value="29">2029</option>
              <option value="30">2030</option>
              <option value="31">2031</option>
            </select>
          </div>
          <div class="form__cvv">
            <p>CVV</p>
            <input
              type="text"
              @focus="cardCvvActive"
              @blur="
                cardCvvAvtive = false;
                showBack = false;
              "
              v-model="cvvNumber"
              v-mask="'###'"
            />
          </div>
        </div>
        <div class="form__submitButton">
          <input type="submit" />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import BancCardBack from "./components/BancCardBack.vue";
import BancCard from "./components/BancCard.vue";
import { mask } from "vue-the-mask";
export default {
  name: "App",
  components: { BancCard, BancCardBack },
  directives: { mask },
  data() {
    return {
      cardNumber: "",
      cardHolder: "",
      expirationMonth: "",
      expirationYear: "",
      cvvNumber: "",
      monthSelectActive: false,
      yearSelectActive: false,
      cardNumberActive: false,
      cardHolderActive: false,
      cardExpirationActive: false,
      cardCvvAvtive: false,
      showBack: false,
    };
  },
  methods: {
    cardCvvActive() {
      this.showBack = true;
      this.cardCvvAvtive = true;
      this.cardNumberActive = false;
      this.cardHolderActive = false;
      this.monthSelectActive = false;
      this.yearSelectActive = false;
    },
    cardType() {
      this.$refs.bancCard.cardType();
    },
    cardNumberFocus() {
      this.cardNumberActive = true;
      this.monthSelectActive = false;
      this.yearSelectActive = false;
      this.cardCvvAvtive = false;
    },
    cardHolderFocus() {
      this.cardHolderActive = true;
      this.monthSelectActive = false;
      this.yearSelectActive = false;
      this.cardCvvAvtive = false;
    },
    cardExpirationMonthFocus() {
      this.cardExpirationActive = true;
      this.monthSelectActive = true;
      if (this.yearSelectActive) {
        this.yearSelectActive = false;
      }
    },
    cardExpirationYearFocus() {
      this.cardExpirationActive = true;
      this.yearSelectActive = true;
      if (this.monthSelectActive) {
        this.monthSelectActive = false;
      }
    },
    focusToCardNumber() {
      this.$refs.cardNumber.focus();
    },
    focusToCardHolder() {
      this.$refs.cardHolder.focus();
    },
    focusToCardMonthExpiration() {
      this.$refs.cardExpirationMonth.focus();
      this.monthSelectActive = true;
      if (this.yearSelectActive) {
        this.yearSelectActive = false;
      }
    },
    focusToCardYearExpiration() {
      this.$refs.cardExpirationYear.focus();
      this.yearSelectActive = true;
      if (this.monthSelectActive) {
        this.monthSelectActive = false;
      }
    },
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Roboto&display=swap");
body {
  font-family: "Roboto", sans-serif;
  margin: 0;
  width: 100%;
  height: 100vh;
}
#app {
  width: 100%;
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  background: #d3e9fc;
}

.form {
  display: flex;
  width: 40%;
  border-radius: 8%;
  bottom: 8%;
  height: 60%;
  position: absolute;
  background: #f8f8f9;
  box-shadow: 0 2.8px 2.2px rgba(0, 0, 0, 0.034),
    0 6.7px 5.3px rgba(0, 0, 0, 0.048), 0 12.5px 10px rgba(0, 0, 0, 0.06),
    0 22.3px 17.9px rgba(0, 0, 0, 0.072), 0 41.8px 33.4px rgba(0, 0, 0, 0.086),
    0 100px 80px rgba(0, 0, 0, 0.12);
  align-items: flex-end;
}
.app__form {
  display: flex;
  flex-direction: column;
  width: 100%;
  height: 65%;
  padding: 0px 15px;
}
.form__cardNumber input {
  height: 45%;
  border-radius: 8px;
  outline: none;
  border-color: #d9dbe3;
  text-transform: uppercase;
  width: 98%;
}
.form__cardHolder input {
  height: 45%;
  border-radius: 8px;
  outline: none;
  border-color: #d9dbe3;
  text-transform: uppercase;
  width: 98%;
}
.form__cardNumber p {
  padding: 0;
  margin: 0;
}
.form__cardHolder p {
  padding: 0;
  margin: 0;
}

.form__cardNumber input[type="number"]::-webkit-inner-spin-button,
input[type="number"]::-webkit-outer-spin-button {
  -webkit-appearance: none;
  margin: 0;
}
.form__cardHolder input[type="number"]::-webkit-inner-spin-button,
input[type="number"]::-webkit-outer-spin-button {
  -webkit-appearance: none;
  margin: 0;
}
.form__cardNumber {
  width: 100%;
  display: flex;
  flex-direction: column;
  height: 25%;
}
.form__cardHolder {
  width: 100%;
  display: flex;
  flex-direction: column;
  height: 25%;
}
.form__lilInput {
  display: flex;
  width: 100%;
  height: 25%;
  justify-content: space-between;
}
.form__lilInput p {
  width: 100%;
  padding: 0;
  margin: 0;
}
.form__date {
  width: 65%;
  display: flex;
  height: 100%;
  flex-wrap: wrap;
  justify-content: space-between;
}
.form__cvv {
  width: 25%;
  display: flex;
  height: 100%;
  flex-wrap: wrap;
  justify-content: space-between;
}
.form__cvv input {
  height: 45%;
  border-radius: 8px;
  outline: none;
  border-color: #d9dbe3;
  text-transform: uppercase;
  width: 100%;
}
.form__cvv input[type="number"]::-webkit-inner-spin-button,
input[type="number"]::-webkit-outer-spin-button {
  -webkit-appearance: none;
  margin: 0;
}
.form__date select {
  width: 45%;
  height: 56%;
  border-radius: 8px;
  outline: none;
}
.form__submitButton {
  width: 100%;
  height: 25%;
}
.form__submitButton input {
  width: 100%;
  height: 64%;
  border-radius: 9px;
  outline: none;
  background: rgb(1, 173, 227);
  color: silver;
  font-family: "Roboto", sans-serif;
  font-size: 21px;
}
.app__form input:focus {
  border: 3px solid rgb(1, 173, 227);
}
.active {
  border: 3px solid rgb(1, 173, 227);
}
</style>
