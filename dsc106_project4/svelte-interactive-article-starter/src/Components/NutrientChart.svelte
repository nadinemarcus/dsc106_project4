<script>
  import { onMount } from 'svelte';
  import * as d3 from 'd3';

  export let nutrients = {};
  export let selectedIngredients = [];

  let chartContainer;

  onMount(() => {
    drawChart();
  });

  // Redraw the chart whenever the nutrients or selectedIngredients prop changes
  $: drawChart();

  function drawChart() {
    if (!chartContainer || !Object.keys(nutrients).length) return;

    // Clear the previous chart
    d3.select(chartContainer).selectAll('*').remove();

    // Set the dimensions and margins of the chart
    const margin = { top: 20, right: 30, bottom: 60, left: 60 },
      width = 600 - margin.left - margin.right,
      height = 400 - margin.top - margin.bottom;

    // Append the SVG object to the div
    const svg = d3.select(chartContainer)
      .append('svg')
      .attr('width', width + margin.left + margin.right)
      .attr('height', height + margin.top + margin.bottom)
      .append('g')
      .attr('transform', `translate(${margin.left},${margin.top})`);

    // X axis
    const x = d3.scaleBand()
      .range([0, width])
      .domain(Object.keys(nutrients))
      .padding(0.2);

    svg.append('g')
      .attr('transform', `translate(0,${height})`)
      .call(d3.axisBottom(x))
      .selectAll('text')
      .attr('transform', 'translate(-10,0)rotate(-45)')
      .style('text-anchor', 'end')
      .style('font-size', '12px');

    // Add Y axis
    const y = d3.scaleLinear()
      .domain([0, 100]) // Percentage scale
      .range([height, 0]);

    svg.append('g')
      .call(d3.axisLeft(y).tickFormat(d => `${d}%`)) // Format ticks as percentages
      .selectAll('text')
      .style('font-size', '12px');

    // Bars
    svg.selectAll('.bar')
      .data(Object.entries(nutrients))
      .enter()
      .append('rect')
      .attr('class', 'bar')
      .attr('x', d => x(d[0]))
      .attr('width', x.bandwidth())
      .attr('y', d => y((d[1] / dailyValues[d[0]]) * 100)) // Calculate percentage
      .attr('height', d => height - y((d[1] / dailyValues[d[0]]) * 100))
      .attr('fill', '#69b3a2');
  }

  // Object containing daily values for each nutrient (adjust values as needed)
  const dailyValues = {
    calories: 2000,
    protein: 50,
    fats: 70,
    carbohydrates: 300
    // Add more nutrients with their corresponding daily values here
  };
</script>

<style>
  .chart-container {
    max-width: 600px;
    margin: auto;
  }

  .bar:hover {
    fill: #4e8fd3;
  }

  .bar-labels {
    font-size: 12px;
  }
</style>

<div class="chart-container" bind:this={chartContainer}></div>
