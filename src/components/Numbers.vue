<template>
  <div>
    <div
      v-for="{ value, isActive } in numbers"
      :key="value"
      class="number"
      :class="{ active: isActive }" 
      :id="'number-' + value"
      @mouseenter="highlighter(value)"
      @mouseleave="reset"
    > 
    <!-- DOM Elements removed because it's not really needed in Vue.Js -->

      {{ value }}
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      numbers: []    
    };
  },

  // Replaced the limit with a required prop
  props: {
    limit: {
      required: true
    }
  },

  // Naming conventions should be closer to what the variable is used for
  watch: {
    limit: {
      handler(newLimit) {
        let numbers = [];
        for (let i = 1; i <= newLimit; i++) {
          numbers.push({ value: i, isActive: false }); // numbers.push is easier to use and allows for cleaner code
        }
        this.numbers = numbers.sort(() => Math.random() - 0.5);
      },
      immediate: true
    }
  },

  methods: {
    highlighter(refValue) {
      this.numbers.forEach((number, index, array) => {
        if (number.value != refValue && refValue % number.value == 0)
          array.splice(index, 1, { value: number.value, isActive: true });
      });
    },


  reset() {
    this.numbers.forEach((number, index, array) => {
      array.splice(index, 1, { value: number.value, isActive: false });
    });
    }
  }
};
</script>

<style scoped>
.number {
  display: inline-block;
  padding: 5px;
  background-color: lightgrey;
  margin: 5px;
  cursor: pointer;
}
.active {
  background-color: black;
  color: #fff;
}
</style>