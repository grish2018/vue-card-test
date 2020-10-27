<template>
  <div class="bancCard" :class="{ transform: showBack }">
    <div class="bancCard__type">
      <div class="type__wrapper">
        <MasterCard v-if="isMasterCard" />
        <Visa v-else-if="isVisa" />
        <AmericanExpress v-else-if="isAmericanExpress" />
        <Discover v-else-if="isDiscover" />
      </div>
    </div>
    <div class="bancCard__number">
      <div class="number__wrapper" @click="cardNumberFocus">
        <Roller
          :text="numberLine"
          :class="{ active: cardNumberActive }"
        ></Roller>
      </div>
    </div>
    <div class="bancCard__info">
      <div class="bancCard__holder">
        <div class="holder__wrapper" @click="cardHolderFocus">
          <Roller
            :text="holderLine.toUpperCase()"
            :class="[{ active: cardHolderActive }, 'nameroller']"
          ></Roller>
        </div>
      </div>
      <div class="bancCard__expires">
        <div
          class="expires__wrapper"
          v-bind:class="{ active: cardExpirationActive }"
        >
          <div
            class="expirationMonthRoller__wrapper"
            @click="cardExpirationMonthFocus"
          >
            <Roller :text="expirationMonthLine" class="nameroller"></Roller>
          </div>
          <span>/</span>
          <div
            class="expirationYearRoller__wrapper"
            @click="cardExpirationYearFocus"
          >
            <Roller :text="expirationYearLine" class="nameroller"></Roller>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Visa from "@/assets/visa.svg";
import MasterCard from "@/assets/masterCard.svg";
import AmericanExpress from "@/assets/americanExpress.svg";
import Discover from "@/assets/discover.svg";
import Roller from "vue-roller";
export default {
  name: "BancCard",
  components: {
    Visa,
    MasterCard,
    AmericanExpress,
    Discover,
    Roller,
  },
  data() {
    return {
      isVisa: true,
      isMasterCard: false,
      isAmericanExpress: false,
      isDiscover: false,
    };
  },
  props: {
    showBack: {
      type: Boolean,
      required: true,
    },
    cardNumber: {
      type: String,
    },
    cardHolder: {
      type: String,
    },
    expirationMonth: {
      type: String,
    },
    expirationYear: {
      type: String,
    },
    cardNumberActive: {
      type: Boolean,
      required: true,
    },
    cardHolderActive: {
      type: Boolean,
      required: true,
    },
    cardExpirationActive: {
      type: Boolean,
      required: true,
    },
  },
  computed: {
    numberLine() {
      if (!this.cardNumber) {
        return `**** **** **** ****`;
      } else {
        const hideNumber = [];
        let cardNumbersArr = [...this.cardNumber];
        for (let i = 0; i < cardNumbersArr.length; i++) {
          if (
            cardNumbersArr[i] &&
            i > 3 &&
            i < 15 &&
            i !== 4 &&
            i !== 9 &&
            i !== 14
          ) {
            cardNumbersArr[i] = "#";
          }
          hideNumber.push(cardNumbersArr[i]);
        }
        return hideNumber.join("");
      }
    },
    holderLine() {
      if (!this.cardHolder) {
        return `GRIGORI SARGSYAN`;
      } else {
        return this.cardHolder;
      }
    },
    expirationMonthLine() {
      if (!this.expirationMonth) {
        return `01`;
      } else {
        return this.expirationMonth;
      }
    },
    expirationYearLine() {
      if (!this.expirationYear) {
        return `24`;
      } else {
        return this.expirationYear;
      }
    },
  },
  methods: {
    cardType() {
      if (this.cardNumber[0] === "4") {
        this.isVisa = true;
        this.isMasterCard = false;
        this.isAmericanExpress = false;
        this.isDiscover = false;
      } else if (this.cardNumber[0] === "5") {
        this.isVisa = false;
        this.isMasterCard = true;
        this.isAmericanExpress = false;
        this.isDiscover = false;
      } else if (
        (this.cardNumber[0] === "3" && this.cardNumber[1] === "4") ||
        this.cardNumber[1] === "7"
      ) {
        this.isVisa = false;
        this.isMasterCard = false;
        this.isAmericanExpress = true;
        this.isDiscover = false;
      } else if (this.cardNumber[0] === "6") {
        this.isVisa = false;
        this.isMasterCard = false;
        this.isAmericanExpress = false;
        this.isDiscover = true;
      } else {
        this.isVisa = true;
        this.isMasterCard = false;
        this.isAmericanExpress = false;
        this.isDiscover = false;
      }
    },
    cardNumberFocus() {
      this.$emit("focusToCardNumber");
    },
    cardHolderFocus() {
      this.$emit("focusToCardHolder");
    },
    cardExpirationMonthFocus() {
      this.$emit("focusToCardMonthExpiration");
    },
    cardExpirationYearFocus() {
      this.$emit("focusToCardYearExpiration");
    },
  },
};
</script>

<style scoped>
@import url("https://fonts.googleapis.com/css2?family=Roboto&display=swap");
.bancCard {
  background-size: cover;
  border-radius: 25px;
  background-image: url(./../assets/super.jpg);
  width: 35%;
  height: 40%;
  position: absolute;
  top: 9%;
  z-index: 9999;
  display: flex;
  flex-direction: column;
  font-family: "Roboto", sans-serif;
}
.bancCard__type {
  width: 100%;
  height: 30%;
  display: flex;
}
.type__wrapper {
  display: flex;
  height: 80%;
  width: 24%;
  margin-top: 3%;
  margin-left: 3%;
}
.type__wrapper svg {
  width: 100%;
  height: 100%;
  color: silver;
}
.bancCard__number {
  width: 100%;
  height: 30%;
  display: flex;
  justify-content: center;
  align-items: center;
}
.number__wrapper {
  width: 90%;
  display: flex;
  height: 100%;
  justify-content: center;
  align-items: center;
}
.roller {
  font-size: 32px;
  color: silver;
  padding: 0% 3%;
}
.nameroller {
  font-size: 26px;
  color: silver;
  padding: 0% 0%;
}
.bancCard__info {
  width: 100%;
  height: 30%;
  display: flex;
}
.bancCard__holder {
  display: flex;
  align-items: center;
  width: 80%;
  height: 100%;
}
.holder__wrapper {
  margin-left: 8%;
}
.bancCard__expires {
  width: 20%;
  height: 100%;
  display: flex;
  align-items: center;
}
.bancCard__info span {
  color: silver;
  font-size: 26px;
}
.expires__wrapper {
  display: flex;
  align-items: baseline;
}
.active {
  border: 2px solid silver;
  border-radius: 6px;
}
.transform {
  animation: cardBack 2s 1;
}
@keyframes cardBack {
  0% {
    transform-origin: center center;
    transform: rotateY(0);
  }
  100% {
    transform-origin: center center;
    transform: rotateY(180deg);
  }
}
</style>
