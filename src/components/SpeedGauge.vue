<template>
  <div class="gauge-container">
    <div ref="speedGauge" class="gauge"></div>
  </div>
</template>

<script>
import * as d3 from 'd3';

export default {
  name: 'SpeedGauge',
  props: {
    value: {
      type: Number,
      required: true
    },
    maxValue: {
      type: Number,
      default: 200 // Maximum speed
    }
  },
  mounted() {
    this.drawGauge();
  },
  watch: {
    value(newValue) {
      this.updateGauge(newValue);
    }
  },
  methods: {
    drawGauge() {
      const width = 400;
      const height = 400;
      const radius = Math.min(width, height) / 2;
      const svg = d3.select(this.$refs.speedGauge)
                    .append('svg')
                    .attr('width', width)
                    .attr('height', height)
                    .append('g')
                    .attr('transform', `translate(${width / 2}, ${height / 2})`);

      const arc = d3.arc()
                    .innerRadius(radius - 30)
                    .outerRadius(radius - 10)
                    .startAngle(-Math.PI / 2)
                    .endAngle(Math.PI / 2);

      svg.append('path')
         .attr('class', 'background-arc')
         .attr('d', arc)
         .attr('fill', '#ccc');

      this.needle = svg.append('line')
                       .attr('class', 'needle')
                       .attr('x1', 0)
                       .attr('y1', 0)
                       .attr('x2', 0)
                       .attr('y2', -(radius - 10))
                       .attr('stroke', '#f00')
                       .attr('stroke-width', 2);

      this.text = svg.append('text')
                     .attr('class', 'gauge-text')
                     .attr('text-anchor', 'middle')
                     .attr('dy', '1em')
                     .attr('font-size', '5em')
                     .text(`${this.value} km/h`);

      this.updateGauge(this.value);
    },
    updateGauge(value) {
      const maxValue = this.maxValue;
      const angleScale = d3.scaleLinear()
                           .domain([0, maxValue])
                           .range([-Math.PI / 2, Math.PI / 2]);

      const angle = angleScale(value);
      const transform = `rotate(${(angle * 180) / Math.PI})`;

      this.needle.transition()
                 .duration(500)
                 .attr('transform', transform);

      this.text.text(`${value} km/h`);
    }
  }
};
</script>

<style scoped>
.gauge-container {
  display: flex;
  flex-direction: column;
  align-items: center;
}
.gauge {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 400px;
  height: 400px;
}
.gauge-text {
  font-size: 1.5em;
  font-weight: bold;
}
</style>