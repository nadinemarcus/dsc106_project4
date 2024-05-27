<script>
  import { onMount } from "svelte";
  import * as d3 from "d3";
  import NutrientChart from "./NutrientChart.svelte";

  let foodData = [];
  let searchQuery = "";
  let searchResults = [];
  let selectedNutrients = {};
  let selectedIngredients = []; // Array to store selected ingredients
  let colorScale = d3.scaleOrdinal(d3.schemeCategory10); // Color scale for ingredients

  onMount(async () => {
    const response = await fetch("/food.csv");
    const data = await response.text();
    foodData = d3.csvParse(data, (d) => ({
      name: d.Description,
      calories: +d["Data.Kilocalories"],
      protein: +d["Data.Protein"],
      fats: +d["Data.Fat.Total Lipid"],
      carbohydrates: +d["Data.Carbohydrate"],
    }));
  });

  // Handle the search functionality
  function handleSearch() {
    searchResults = foodData.filter((food) =>
      food.name.toLowerCase().includes(searchQuery.toLowerCase()),
    );
  }

  // Select a food item to display its nutrients
  function selectFood(food) {
    selectedNutrients = {
      calories: food.calories,
      protein: food.protein,
      fats: food.fats,
      carbohydrates: food.carbohydrates,
    };
    selectedIngredients.push(food); // Add selected food to the array
    searchQuery = ""; // Clear the search bar
    searchResults = [];
  }
</script>

<div class="search-container">
  <input
    type="text"
    placeholder="Search for food..."
    bind:value={searchQuery}
    on:input={handleSearch}
  />
  <ul>
    {#each searchResults as result}
      <li on:click={() => selectFood(result)}>
        <strong>{result.name}</strong><br />
        Calories: {result.calories}, Protein: {result.protein}g, Fats: {result.fats}g,
        Carbohydrates: {result.carbohydrates}g
      </li>
    {/each}
  </ul>
  {#if Object.keys(selectedNutrients).length > 0}
    <NutrientChart
      nutrients={selectedNutrients}
      ingredients={selectedIngredients}
      {colorScale}
    />
  {/if}
</div>

<style>
  .search-container {
    padding: 20px;
    max-width: 600px;
    margin: auto;
  }
  input[type="text"] {
    width: 100%;
    padding: 10px;
    margin-bottom: 20px;
    font-size: 16px;
    border: 1px solid #ccc;
    border-radius: 4px;
  }
  ul {
    list-style: none;
    padding: 0;
  }
  li {
    padding: 10px;
    border-bottom: 1px solid #ddd;
    cursor: pointer;
  }
</style>
