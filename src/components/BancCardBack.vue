<template>
  <div class="bancCard" :class="{ transform: showBack }">
    <div class="bancCard__magniseLine"></div>
    <div class="cvv__wrapper">
      <div class="cvv__label">
        <span>CVV</span>
      </div>
      <div class="cvv__space">
        <Roller :text="cvvLine" class="cvv__roller"></Roller>
      </div>
    </div>
  </div>
</template>

<script>
import Roller from "vue-roller";
export default {
  name: "BancCardBack",
  components: {
    Roller,
  },
  props: {
    cvvNumber: {
      type: String,
    },
    showBack: {
      type: Boolean,
    },
  },
  computed: {
    cvvLine() {
      const hideCvv = [];
      let cardCvvArr = [...this.cvvNumber];
      for (let i = 0; i < cardCvvArr.length; i++) {
        cardCvvArr[i] = "#";
        hideCvv.push(cardCvvArr[i]);
      }
      return hideCvv.join("");
    },
  },
};
</script>

<style scoped>
@import url("https://fonts.googleapis.com/css2?family=Roboto&display=swap");
.bancCard {
  background-size: cover;
  border-radius: 25px;
  background: rgb(32, 53, 79);
  width: 35%;
  height: 40%;
  position: absolute;
  top: 9%;
  z-index: 9999;
  display: flex;
  flex-direction: column;
  font-family: "Roboto", sans-serif;
}
.bancCard__magniseLine {
  width: 100%;
  height: 15%;
  margin-top: 11%;
  background: silver;
}
.cvv__wrapper {
  display: flex;
  flex-direction: column;
  margin-top: 13%;
  justify-content: center;
  align-items: center;
  height: 23%;
  width: 100%;
}
.cvv__label {
  display: flex;
  width: 87%;
  color: silver;
  justify-content: flex-end;
  font-size: 23px;
}
.cvv__space {
  display: flex;
  width: 90%;
  height: 100%;
  background: white;
  border-radius: 8px;
  justify-content: flex-end;
}
.cvv__roller {
  display: flex;
  align-items: center;
  margin-right: 3%;
  font-size: 23px;
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