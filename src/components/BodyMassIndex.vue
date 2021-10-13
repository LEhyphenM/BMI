<template>
  <div>
    <div v-text="bmi" />
    <div v-text="bmiClass" />
  </div>
</template>

<script>
export default {
  name: "BodyMassIndex",
  props: {
    height: {
      type: Number,
      required: false,
      default: 0
    },
    mass: {
      type: Number,
      required: false,
      default: 0
    },
    system: {
      type: String,
      required: false,
      default: 'imperial'
    }
  },
  methods: {
    calculateMetricBMI(mass, height) {
      const targetHeight = height / 100;
      const bmi = mass / (targetHeight * targetHeight);
      return Math.round(bmi) / 100;
    },
    calculateImperialBMI(mass, height) {
      const bmi = 703 * (mass / (height * height));
      return Math.round(bmi) / 100;
    }
  },
  computed: {
    bmi() {
      switch (this.system) {
        case 'imperial':
          return this.calculateImperialBMI(this.mass, this.height);
        case 'metric':
          return this.calculateMetricBMI(this.mass, this.height);
        default:
          return null;
      }
    },
    bmiClass() {
      if (!this.bmi) return null;
      if (this.bmi < 18.5) {
        return "Underweight";
      } else if (this.bmi < 25) {
        return "Normal weight";
      } else if (this.bmi < 30) {
        return "Pre-obesity";
      } else if (this.bmi < 35) {
        return "Obesity class I";
      } else if (this.bmi < 40) {
        return "Obesity class II";
      } else {
        return "Obesity class III";
      }
    }
  }
}
</script>