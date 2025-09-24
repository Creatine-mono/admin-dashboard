<template>
  <div ref="chart" class="d3-chart"></div>
</template>

<script>
import * as d3 from 'd3';

export default {
  name: 'IpAccessChart',
  props: {
    chartData: {
      type: Array,
      default: () => [
        { ip: '192.168.1.10', requests: 150, blocked: 15, ratio: 10 },
        { ip: '192.168.1.15', requests: 120, blocked: 8, ratio: 6.7 },
        { ip: '192.168.1.20', requests: 90, blocked: 12, ratio: 13.3 },
        { ip: '192.168.1.25', requests: 75, blocked: 5, ratio: 6.7 },
        { ip: '192.168.1.30', requests: 60, blocked: 3, ratio: 5 }
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

      const margin = { top: 20, right: 30, bottom: 80, left: 50 };
      const width = container.clientWidth - margin.left - margin.right;
      const height = 300 - margin.top - margin.bottom;

      const svg = d3.select(container)
        .append('svg')
        .attr('width', width + margin.left + margin.right)
        .attr('height', height + margin.top + margin.bottom);

      const g = svg.append('g')
        .attr('transform', `translate(${margin.left},${margin.top})`);

      const xScale = d3.scaleBand()
        .domain(this.chartData.map(d => d.ip))
        .range([0, width])
        .padding(0.2);

      const yScale = d3.scaleLinear()
        .domain([0, d3.max(this.chartData, d => d.requests)])
        .range([height, 0]);

      g.append('g')
        .attr('transform', `translate(0,${height})`)
        .call(d3.axisBottom(xScale))
        .selectAll('text')
        .style('text-anchor', 'end')
        .attr('dx', '-.8em')
        .attr('dy', '.15em')
        .attr('transform', 'rotate(-45)')
        .style('font-size', '12px');

      g.append('g')
        .call(d3.axisLeft(yScale))
        .style('font-size', '12px');

      const tooltip = d3.select('body').append('div')
        .attr('class', 'tooltip')
        .style('position', 'absolute')
        .style('visibility', 'hidden')
        .style('background-color', 'rgba(0, 0, 0, 0.8)')
        .style('color', 'white')
        .style('padding', '10px')
        .style('border-radius', '5px')
        .style('font-size', '12px')
        .style('z-index', '1000');

      g.selectAll('.bar-requests')
        .data(this.chartData)
        .enter().append('rect')
        .attr('class', 'bar-requests')
        .attr('x', d => xScale(d.ip))
        .attr('width', xScale.bandwidth() / 2)
        .attr('y', d => yScale(d.requests))
        .attr('height', d => height - yScale(d.requests))
        .attr('fill', '#007bff')
        .attr('opacity', 0.7)
        .on('mouseover', function(event, d) {
          tooltip.style('visibility', 'visible')
            .text(`${d.ip}: ${d.requests}회 요청`);
        })
        .on('mousemove', function(event) {
          tooltip.style('top', (event.pageY - 10) + 'px')
            .style('left', (event.pageX + 10) + 'px');
        })
        .on('mouseout', function() {
          tooltip.style('visibility', 'hidden');
        });

      g.selectAll('.bar-blocked')
        .data(this.chartData)
        .enter().append('rect')
        .attr('class', 'bar-blocked')
        .attr('x', d => xScale(d.ip) + xScale.bandwidth() / 2)
        .attr('width', xScale.bandwidth() / 2)
        .attr('y', d => yScale(d.blocked))
        .attr('height', d => height - yScale(d.blocked))
        .attr('fill', '#dc3545')
        .attr('opacity', 0.8)
        .on('mouseover', function(event, d) {
          tooltip.style('visibility', 'visible')
            .text(`${d.ip}: ${d.blocked}회 차단 (${d.ratio.toFixed(1)}%)`);
        })
        .on('mousemove', function(event) {
          tooltip.style('top', (event.pageY - 10) + 'px')
            .style('left', (event.pageX + 10) + 'px');
        })
        .on('mouseout', function() {
          tooltip.style('visibility', 'hidden');
        });

      const legend = g.append('g')
        .attr('transform', `translate(${width - 100}, 20)`);

      legend.append('rect')
        .attr('x', 0)
        .attr('y', 0)
        .attr('width', 15)
        .attr('height', 15)
        .attr('fill', '#007bff')
        .attr('opacity', 0.7);

      legend.append('text')
        .attr('x', 20)
        .attr('y', 12)
        .text('요청')
        .style('font-size', '12px')
        .attr('fill', '#333');

      legend.append('rect')
        .attr('x', 0)
        .attr('y', 20)
        .attr('width', 15)
        .attr('height', 15)
        .attr('fill', '#dc3545')
        .attr('opacity', 0.8);

      legend.append('text')
        .attr('x', 20)
        .attr('y', 32)
        .text('차단')
        .style('font-size', '12px')
        .attr('fill', '#333');
    }
  },
  beforeUnmount() {
    d3.selectAll('.tooltip').remove();
  }
}
</script>

<style scoped>
.d3-chart {
  width: 100%;
  height: 300px;
}
</style>