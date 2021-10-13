<template>
  <form>
    <fieldset>
      <div class="heightInput">
        <label>{{ ftLabel }}</label>
        <input v-model.number="height" placeholder="0" />
        <label>{{ inLabel }}</label>
        <input v-model.number="inches" placeholder="0" />
      </div>

      <div>
        <label>{{ massLabel }}</label>
        <input v-model.number="mass" placeholder="0" />
      </div>
    </fieldset>
  </form>
  <div v-bind:class="{ active : isActive, hide : isHidden }" class="bmiDisplay" >
    <span class="bmiTitle"> {{ bmiLabel }} {{ bmi }} </span>
    <span class="bmiSubtext" v-bind:class="{ under : isUnder, normal : isNorm, over : isOver, obsese1 : isObsese1, obsese2 : isObsese2, obsese3 : isObsese3,  }"> {{ bmiClass }} </span>
  </div>
</template>

<script>
export default {
  name: "Form",
  props: {
    height: {
      type: Number,
      required: false,
      default: 0
    },
    inches: {
      type: Number,
      required: false,
      default: 0
    },
    mass: {
      type: Number,
      reuired: false,
      default: 0
    },
    system: {
      type: String,
      required: false,
      default: "imperial",
    }
  },
  data() {
    return {
      ftLabel: "Feet",
      inLabel: "Inches",
      massLabel: "Mass in LB",
      // submitBtn: "Calculate",
      isActive: false,
      isHidden: true,
      bmiLabel: "Your BMI is: ",
      isUnder: false,
      isNorm: false,
      isOver: false,
      isObsese1: false,
      isObsese2: false,
      isObsese3: false,
    }
  },
  methods: {
    // calculateMetricBMI(mass, height) {
      // const inchPercent = inches / 12;
      // const targetHeight = (height + inchPercent) / 3.2808; //3.2808
      // const kgMass = mass * 0.45359237;
      // const kgMass = mass / 2.2046;

    //   const targetHeight = height * height;
    //   const bmi = mass / targetHeight;
    //   return Math.round(bmi) * 100;
    // },
    calculateImperialBMI(mass, height, inches) {
      const calcHeight = (height * 12) + inches;
      const targetHeight = calcHeight ** 2;
      const bmi = (mass / targetHeight) * 703;
      return Math.round(10 * bmi) / 10; //round to nearest tenth
    }
  },
  computed: {
    // bmi() {
      // switch (this.system) {
        // case 'imperial':
          // return this.calculateImperialBMI(this.mass, this.height, this.inches);
        // case 'metric':
        //   return this.calculateMetricBMI(this.mass, this.height, this.inches);
        // default:
          // return null;
    // },
    bmi() {
      switch (this.system) {
        case 'imperial': 
          return this.calculateImperialBMI(this.mass, this.height, this.inches);
        default:
          return null;
      }
    },

    bmiClass() {
      if (!this.bmi) return null;

      if (this.bmi < 15) {
        this.isActive = true;
        this.isHidden = false;
        return "Critically Underweight";
      
      } else if (this.bmi >= 15 && this.bmi < 16) {
          return "Severely Underweight";

      } else if(this.bmi >= 16 && this.bmi < 18.5) {
        this.isUnder = true;
        return "Underweight"

      } else if (this.bmi < 25) {
        this.isUnder = false;
        this.isNorm = true;
        return "Healthy Weight";

      } else if (this.bmi < 30) {
        this.isNorm = false;
        this.isOver = true;
        return "Overweight";

      } else if (this.bmi < 35) {
        this.isOver = false;
        this.isObsese1 = true;
        return "Moderately Obsese";

      } else if (this.bmi < 40) {
        this.isObsese1 = false;
        this.isObsese2 = true;
        return "Severely Obese";

      } else if (this.bmi > 40) {
        this.isObsese2 = false;
        this.isObsese3 = true;
        return "Critically Obese";
      }
    }
  }
}
</script>

<style lang="scss">
  @import "../scss/variables.scss";
  @import "../scss/mixins.scss";
  .noInches {opacity:0; display: none;}
  form {
    display:flex;
    flex-direction: column;
    width: 100%;
    fieldset{
      display:grid;
      border-radius:$size1;
      border:1px solid $primary;
      padding:60px 0px;
      margin:0 auto;
      width:100%;
    }
    div {
      margin:0px auto;
      margin:0 auto;
      label {
        @include defaultText();
        font-size: 14px;
        font-weight: bold;
        display:flex;
        text-indent:6px;
        line-height:15px;
      }
    }
    input,
    button {
      @include styleRemoval();
      font-size: 18px;
      padding-left: $size2;
      padding-right: $size2;
      border-radius: $size1;
      margin-top: $size1;
      margin-bottom: $size2;
      min-width:300px;
    }
    input {
      height: $size5;
      background-color: transparent;
      border:1px solid $text;
      color: inherit;
      &:focus {
        background-color:darken($background, 6%);
        transition: all 0.65s ease;
      }
    }
    input::placeholder {
      color:$text;
    }
  }

  .hide {
    opacity:0;
  }

  .active {
    opacity:1;
    transition: all 0.65s ease;
  }
  
  .bmiDisplay {
    display:grid;
    border-radius:$size1;
    margin-top:20px;
    margin-bottom:29px;
    border: 1px solid $primary;
    padding:30px 40px;
    line-height:$size4;

    span { 
      @include bmiClass();
    }
    .bmiTitle {
      font-weight:bold;
      font-size:22px;
    }
    .bmiSubtext {
      font-size:16px;
    }

    // .under{background-color:$under;}
    // .normal{background-color:none;}
    // .over{background-color:$over;}
    // .obsese1 {background-color:$obsese1;}
    // .obsese2 {background-color:$obsese2;}
    // .obsese3 {background-color:$obsese3;}
  }

  // X-Small devices (portrait phones, less than 576px)
  @media (max-width: 575.98px) {
    form > fieldset {
      padding: 50px 0;

      input {
        min-width:250px;
      }
    }
    .bmiDisplay {
      padding: 30px 40px;
    }
  }
</style>