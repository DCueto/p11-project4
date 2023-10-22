<script>
  import List from "./List.svelte";
  import Hero from './Hero.svelte';

  // const titles = {
  //   lista1: {
  //     fetchUrl: "https://api.themoviedb.org/3/movie/popular?language=en-US&page=1",
  //     title: "Películas Destacadas"
  //   },

  //   title1: 'Películas destacadas',
  // }


</script>

<main>

  <Hero fetchUrl="https://api.themoviedb.org/3/movie/top_rated?language=en-US&page=1"/>

  <List fetchUrl="https://api.themoviedb.org/3/movie/popular?language=en-US&page=1" title="Peliculas Destacadas"/>

  <List fetchUrl="https://api.themoviedb.org/3/movie/upcoming" title="Películas por llegar "/>
</main>

<style>

  main{
    width: 100%;
  }

</style>