<template>
  <div ref="chart" class="d3-chart"></div>
</template>

<script>
import * as d3 from 'd3';

export default {
  name: 'QuarterlyLeakChart',
  props: {
    chartData: {
      type: Array,
      default: () => [
        { label: '전화번호', q1: 45, q2: 38, q3: 52, q4: 41 },
        { label: '주민등록번호', q1: 23, q2: 18, q3: 29, q4: 25 },
        { label: '신용카드번호', q1: 15, q2: 12, q3: 18, q4: 14 },
        { label: '이메일주소', q1: 32, q2: 28, q3: 35, q4: 30 },
        { label: '계좌번호', q1: 8, q2: 6, q3: 11, q4: 9 }
      ]
    }
  },
  mounted() {
    this.drawChart();
  },
  watch: {
    chartData: {
      handler: 'drawChart',
      deep: true
    }
  },
  methods: {
    drawChart() {
      const container = this.$refs.chart;
      d3.select(container).selectAll("*").remove();

      const margin = { top: 20, right: 80, bottom: 80, left: 80 };
      const width = container.clientWidth - margin.left - margin.right;
      const height = 300 - margin.top - margin.bottom;

      const svg = d3.select(container)
        .append('svg')
        .attr('width', width + margin.left + margin.right)
        .attr('height', height + margin.top + margin.bottom);

      const g = svg.append('g')
        .attr('transform', `translate(${margin.left},${margin.top})`);

      // Transform data for stacked bar chart
      const quarters = ['q1', 'q2', 'q3', 'q4'];
      const stack = d3.stack()
        .keys(quarters)
        .order(d3.stackOrderNone)
        .offset(d3.stackOffsetNone);

      const stackedData = stack(this.chartData);

      const xScale = d3.scaleBand()
        .domain(this.chartData.map(d => d.label))
        .range([0, width])
        .padding(0.1);

      const yScale = d3.scaleLinear()
        .domain([0, d3.max(stackedData, d => d3.max(d, d => d[1]))])
        .nice()
        .range([height, 0]);

      const colorScale = d3.scaleOrdinal()
        .domain(quarters)
        .range(['#ff6b6b', '#4ecdc4', '#45b7d1', '#96ceb4']);

      // Create tooltip
      const tooltip = d3.select('body').append('div')
        .attr('class', 'quarterly-tooltip')
        .style('position', 'absolute')
        .style('visibility', 'hidden')
        .style('background-color', 'rgba(0, 0, 0, 0.8)')
        .style('color', 'white')
        .style('padding', '10px')
        .style('border-radius', '5px')
        .style('font-size', '12px')
        .style('z-index', '1000');

      // Draw stacked bars
      g.selectAll('.layer')
        .data(stackedData)
        .enter().append('g')
        .attr('class', 'layer')
        .attr('fill', d => colorScale(d.key))
        .selectAll('rect')
        .data(d => d)
        .enter().append('rect')
        .attr('x', d => xScale(d.data.label))
        .attr('y', d => yScale(d[1]))
        .attr('height', d => yScale(d[0]) - yScale(d[1]))
        .attr('width', xScale.bandwidth())
        .on('mouseover', function(event, d) {
          const quarter = d3.select(this.parentNode).datum().key;
          const quarterName = {
            'q1': '1분기',
            'q2': '2분기',
            'q3': '3분기',
            'q4': '4분기'
          }[quarter];
          const value = d[1] - d[0];

          tooltip.style('visibility', 'visible')
            .html(`${d.data.label}<br/>${quarterName}: ${value}건`);
        })
        .on('mousemove', function(event) {
          tooltip.style('top', (event.pageY - 10) + 'px')
            .style('left', (event.pageX + 10) + 'px');
        })
        .on('mouseout', function() {
          tooltip.style('visibility', 'hidden');
        });

      // Add X axis
      g.append('g')
        .attr('transform', `translate(0,${height})`)
        .call(d3.axisBottom(xScale))
        .selectAll('text')
        .style('text-anchor', 'end')
        .attr('dx', '-.8em')
        .attr('dy', '.15em')
        .attr('transform', 'rotate(-45)')
        .style('font-size', '11px');

      // Add Y axis
      g.append('g')
        .call(d3.axisLeft(yScale))
        .style('font-size', '12px');

      // Add Y axis label
      g.append('text')
        .attr('transform', 'rotate(-90)')
        .attr('y', 0 - margin.left)
        .attr('x', 0 - (height / 2))
        .attr('dy', '1em')
        .style('text-anchor', 'middle')
        .style('font-size', '12px')
        .text('유출 건수');

      // Add legend
      const legend = g.append('g')
        .attr('transform', `translate(${width + 10}, 20)`);

      const legendItems = legend.selectAll('.legend-item')
        .data(quarters)
        .enter().append('g')
        .attr('class', 'legend-item')
        .attr('transform', (d, i) => `translate(0, ${i * 20})`);

      legendItems.append('rect')
        .attr('x', 0)
        .attr('y', 0)
        .attr('width', 15)
        .attr('height', 15)
        .attr('fill', d => colorScale(d));

      legendItems.append('text')
        .attr('x', 20)
        .attr('y', 12)
        .text(d => {
          const quarterNames = {
            'q1': '1분기',
            'q2': '2분기',
            'q3': '3분기',
            'q4': '4분기'
          };
          return quarterNames[d];
        })
        .style('font-size', '12px')
        .attr('fill', '#333');
    }
  },
  beforeUnmount() {
    d3.selectAll('.quarterly-tooltip').remove();
  }
}
</script>

<style scoped>
.d3-chart {
  width: 100%;
  height: 300px;
}
</style>