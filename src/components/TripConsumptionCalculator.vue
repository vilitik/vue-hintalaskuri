<template>
  <verStack color="white">
    <img
      v-if="endPosInfo && endPosInfo.wikipedia && endPosInfo.wikipedia.image"
      :src="endPosInfo.wikipedia.image"
    >
    <cSelect
      v-if="!tripLength && !loadingRouteLen"
      v-model="travelType"
      label="Matkan tyyppi"
      :items="['Yksi suunta', 'Edestakainen']"
    />
    <horStack v-if="!tripLength && !loadingRouteLen">
      <cInput v-model="startPos" label="Lähtöpaikka" placeholder="Kiuruvesi"/>
      <cInput @blur="fetchRouteLen" v-model="endPos" label="Kohde" placeholder="Iisalmi"/>
    </horStack>
    <p v-show="loadingRouteLen">Odota, laskee etäisyyttä...</p>
    <cInput
      v-if="tripLength"
      v-model="tripLength"
      validate="decimal"
      label="Matkan pituus"
      suffix=" km"
    />
    <cInput v-model="consumption" validate="decimal" label="Auton kulutus (l/100km)"/>
    <cSelect v-model="chosenFuel" :items="fuels" label="Valitse auton polttoaine"/>
    <bigNumber label="Matkan hinta" :num="totalPrice"/>
    <a @click="clear" class="clear">Tyhjennä</a>
  </verStack>
</template>

<script>
import verStack from "@/components/part/verStack";
import horStack from "@/components/part/horStack";
import cInput from "@/components/part/cInput";
import cSelect from "@/components/part/cSelect";
import bigNumber from "@/components/part/bigNumber";

export default {
  data() {
    return {
      fuels: ["Diesel", "95 E10", "98 E5"],
      /*fuels: [
        { text: "Diesel", price: "1.264" },
        { text: "95 E10", price: "1.461" },
        { text: "98 E5", price: "1.546" }
      ],*/
      chosenFuel: localStorage.getItem("calcChosenFuel") || null,
      fuelPrice: null,
      tripLength: "",
      consumption: localStorage.getItem("calcConsumption") || null,
      totalPrice: null,
      startPos: null,
      endPos: null,
      travelType: "Edestakainen",
      endPosInfo: {},
      loadingRouteLen: false
    };
  },
  components: {
    verStack,
    horStack,
    cInput,
    cSelect,
    bigNumber
  },
  methods: {
    clear() {
      localStorage.removeItem("calcConsumption");
      localStorage.removeItem("calcChosenFuel");
      this.tripLength = null;
      this.startPos = null;
      this.endPos = null;
      this.consumption = null;
      this.endPosInfo = {};
      this.travelType = "Edestakainen";
    },
    calculate() {
      this.calculateFuelPrice();
      if (!this.consumption || !this.tripLength || !this.fuelPrice)
        return (this.totalPrice = null);
      this.totalPrice =
        "~" +
        this.round(
          (this.consumption / 100) * this.tripLength * this.fuelPrice
        ) +
        "€";
    },
    calculateFuelPrice() {
      let fuelPrice = null;
      switch (this.chosenFuel) {
        case "Diesel":
          fuelPrice = 1.264;
          break;
        case "95 E10":
          fuelPrice = 1.461;
          break;
        case "98 E5":
          fuelPrice = 1.546;
          break;

        default:
          break;
      }
      this.fuelPrice = fuelPrice;
    },
    round(val) {
      return +(Math.round(val + "e+2") + "e-2");
    },
    fetchRouteLen() {
      if (!this.startPos || !this.endPos) return;
      this.loadingRouteLen = true;
      fetch(
        `https://cors-anywhere.herokuapp.com/https://www.vaelimatka.org/route.json?stops=${
          this.startPos
        }|${this.endPos}`
      )
        .then(res => res.json())
        .then(res => {
          this.travelType === "Edestakainen"
            ? (this.tripLength = res.distance * 2)
            : (this.tripLength = res.distance);
          this.endPosInfo = res.stops[1];
          this.loadingRouteLen = false;
          console.log(res);
        });
    }
  },
  watch: {
    tripLength() {
      this.calculate();
    },
    consumption(val) {
      if (val) localStorage.setItem("calcConsumption", val);
      this.calculate();
    },
    chosenFuel(val) {
      if (val) localStorage.setItem("calcChosenFuel", val);
      this.calculate();
    }
  }
};
</script>
<style scoped>
img {
  position: fixed;
  margin: 0 !important;
  padding: 0 !important;
  top: 0;
  left: 0;
  width: 100%;
  height: auto;
  z-index: -999;
}
</style>

