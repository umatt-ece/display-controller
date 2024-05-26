<template>
  <div class="temperature-bar">
    <div class="fill" :style="fillStyle"></div>
    <div class="label">{{ description }}: {{ value }}°C</div>
  </div>
</template>

<script>
export default {
  name: 'TemperatureBar',
  props: {
    value: {
      type: Number,
      required: true
    },
    maxValue: {
      type: Number,
      default: 100 // Maximum temperature
    },
    minValue: {
      type: Number,
      default: 0 // Minimum temperature
    },
    description: {
      type: String,
      default: 'Temperature'
    }
  },
  computed: {
    fillStyle() {
      const percentage = ((this.value - this.minValue) / (this.maxValue - this.minValue)) * 100;
      return {
        width: `${percentage}%`,
        backgroundColor: this.getColor(this.value)
      };
    }
  },
  methods: {
    getColor(value) {
      if (value <= 20) {
        return 'blue';
      } else if (value <= 50) {
        return 'green';
      } else if (value <= 80) {
        return 'yellow';
      } else {
        return 'red';
      }
    }
  }
};
</script>

<style scoped>
.temperature-bar {
  width: 1000px;
  height: 50px;
  border: 1px solid #000;
  position: relative;
  background-color: #eee;
}
.fill {
  width: 0;
  height: 100%;
  transition: width 0.5s, background-color 0.5s;
}
.label {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  font-size: 36px;
  color: #000;
}
</style>