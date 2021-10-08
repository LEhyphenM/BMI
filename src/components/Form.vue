<template>
  <form @submit.prevent="calculate">
    <fieldset>
      <div>
        <label>{{ heightLabel }}</label>
        <input v-model.number="height" placeholder="0" />
      </div>

      <div>
        <label>{{ massLabel }}</label>
        <input v-model.number="mass" placeholder="0" />
      </div>

      <button type="submit">{{ submitBtn }}</button>
    </fieldset>
  </form>
  
  <p>{{ bmiLabel }} {{ bmi }}</p>
  
</template>

<script>
export default {
  name: "Form",
  data() {
    return {
      heightLabel: "Height in Meters",
      height: "",
      massLabel: "Mass in KG",
      mass: "",
      submitBtn: "Calculate",
      bmiLabel: "BMI: ",
      bmi: 0,
    };
  },
  computed: {
    formValid() {
      const { height, mass } = this;
      return +height > 0 && +mass > 0;
    },
  },
  methods: {
    calculate() {
      if (!this.formValid) {
        return;
      }
      const { height, mass } = this;
      this.bmi = mass / height ** 2;

      if(!this.height, !this.mass) return;
      this.$emit('on-submit', this.height);
      this.$emit('on-submit', this.mass);
      console.log(this.height);
      console.log(this.mass);
      console.log(this.bmi);
    }
  },
};
</script>

<style lang="scss">
  @import "../scss/variables.scss";
  @import "../scss/mixins.scss";
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
    button {
      height:$size6;
      color:$text;
      background-color: $primary;
      border:1px solid $primary;
      font-weight: bold;
      outline: none;
      border-radius: $size1;
      margin-top:30px;
      margin-left:auto;
      margin-right:auto;
      max-width:100%;
      min-width:300px;
      &:hover {
        cursor:pointer;
        background-color: lighten($primary, 3%);
        transition: all 0.65s ease;
      }
    }
  }
  p { 
    color:$text;
    font-weight:bold;
  }
</style>