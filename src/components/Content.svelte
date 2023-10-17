<script>
  import Title from "./Title.svelte";
  import Movie from "./Movie.svelte";

  import p1 from "./../assets/figma/Posters/poster_0_3_210x0-1.webp";
  import p2 from "./../assets/figma/Posters/poster_0_3_210x0-2.webp";
  import p3 from "./../assets/figma/Posters/poster_0_3_210x0-3.webp";
  import p4 from "./../assets/figma/Posters/poster_0_3_210x0-4.webp";


  const titles = {
    title1: 'Películas destacadas',
  }


  const options = {
    method: 'GET',
    headers: {
      accept: 'application/json',
      Authorization: 'Bearer eyJhbGciOiJIUzI1NiJ9.eyJhdWQiOiIxM2FkNzIwZWM4NTY2NjNjNWI0ZjFkODk3MzI3OWQwMyIsInN1YiI6IjY1MmQwMGNkMWYzZTYwMDEzOTlmOGU1YiIsInNjb3BlcyI6WyJhcGlfcmVhZCJdLCJ2ZXJzaW9uIjoxfQ.mbSJcAq51x8-kXgJEdKY_1asnZc7RW4sBIl-B-N9qzU'
    }
  };

  let movies = [];

  fetch('https://api.themoviedb.org/3/movie/popular?language=en-US&page=1', options)
    .then(response => response.json())
    .then(response => {
      console.log(response)

      movies = response.results.map(movie => {
        const movies = {
            id: movie.id,
            title: movie.title,
            lang: movie.original_language,
            img_src: "https://image.tmdb.org/t/p/w500" + movie.poster_path
          
        };
        return movies;
      });
      console.log(movies);
    })
    .catch(err => console.error(err));


</script>


<main id="content">
  <Title text={titles.title1}/>
  <section class="movies">
    {#each movies as movie}
      <Movie imageSrc={movie.img_src} />
    {/each}
  </section>
</main>

<style>
  
  main#content{
    width: 100%;
    padding: 150px 64px;
  }

  section.movies{
    margin: 30px 0;
    display: flex;
    gap: 30px;
    flex-wrap: wrap;
  }
</style>