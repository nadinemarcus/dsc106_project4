<script>
  import Meta from "./Meta.svelte";
  import Title from "./Components/Title.svelte";
  import Intro from "./Components/Intro.svelte";
  import TextAndMathEquations from "./Components/TextAndMathEquations.svelte";
  import LineChart from "./Components/LineChart.svelte";
  import ScrollCenter from "./Components/ScrollCenter.svelte";
  import Conclusion from "./Components/Conclusion.svelte";
  import Resources from "./Components/Resources.svelte";
  import ScrollSide from "./Components/ScrollSide.svelte";
  import FoodSearch from "./Components/FoodSearch.svelte";
</script>

<main>
  <h1>Nutrient Impact Explorer</h1>
  <FoodSearch />
</main>

<style>
  /* Add your styles here */
</style>

<Meta />
<Title />
<Intro />
<TextAndMathEquations />
<ScrollSide />
<LineChart />
<ScrollCenter />
<Conclusion />
<Resources />
