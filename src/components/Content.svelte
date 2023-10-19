<script>
  import Title from "./Title.svelte";
  import Movie from "./Movie.svelte";
  import List from "./List.svelte";

  import p1 from "./../assets/figma/Posters/poster_0_3_210x0-1.webp";
  import p2 from "./../assets/figma/Posters/poster_0_3_210x0-2.webp";
  import p3 from "./../assets/figma/Posters/poster_0_3_210x0-3.webp";
  import p4 from "./../assets/figma/Posters/poster_0_3_210x0-4.webp";


  const titles = {
    title1: 'Películas destacadas',
  }


</script>


<main id="content">
  <List fetchUrl="https://api.themoviedb.org/3/movie/popular?language=en-US&page=1" title="Peliculas Destacadas"/>
</main>

<style>

  main#content{
    width: 100%;
    padding: 200px 0;
  }

</style>