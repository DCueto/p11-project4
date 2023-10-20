<script>
  import Movie from "./Movie.svelte";
  import Title from "./Title.svelte";

  import { onMount } from 'svelte';

  export let fetchUrl, title;

  let moviesContainerEl, moviesSlider, leftBtn, rightBtn;

  let viewportWidth;

  let movies = [];

  onMount(async () => {
    const options = {
      method: 'GET',
      headers: {
        accept: 'application/json',
        Authorization: 'Bearer eyJhbGciOiJIUzI1NiJ9.eyJhdWQiOiIxM2FkNzIwZWM4NTY2NjNjNWI0ZjFkODk3MzI3OWQwMyIsInN1YiI6IjY1MmQwMGNkMWYzZTYwMDEzOTlmOGU1YiIsInNjb3BlcyI6WyJhcGlfcmVhZCJdLCJ2ZXJzaW9uIjoxfQ.mbSJcAq51x8-kXgJEdKY_1asnZc7RW4sBIl-B-N9qzU'
      }
    };
    
    const request = await fetch(fetchUrl, options);
    const response = await request.json();

    //console.log(response);

    if(request.ok){
      // console.log(response);

      movies = response.results.map(movie => {
        const movies = {
            id: movie.id,
            title: movie.title,
            lang: movie.original_language,
            img_src: "https://image.tmdb.org/t/p/w500" + movie.poster_path

        };
        return movies;
      });
      // console.log(movies);
    } else{
      // console.log(response);
    }

  });
  

    /* EVENT FUNCTIONS */
  
  function widthResized(e){
    viewportWidth = moviesSlider.offsetWidth;
  }

  function slideRight(event){
    event.stopPropagation();
    widthResized();

    // console.log(moviesContainerEl);
    moviesContainerEl.scrollBy({
      top: 0,
      left: viewportWidth,
      behavior: "smooth",
    });

    // CAMBIAR LOS DISPLAYS NO FUNCIONA BIEN
    
    setTimeout(()=>{
      if(moviesContainerEl.scrollLeft + moviesContainerEl.offsetWidth >= moviesContainerEl.scrollWidth){
        console.log("SCROLL MAX " + moviesContainerEl.scrollWidth);
        //rightBtn.style.display = "none";
      } else{
        //rightBtn.style.display = "flex";
      }

    }, 1000);
  }

  function slideLeft(event){
    event.stopPropagation();
    widthResized();
    moviesContainerEl.scrollBy({
      top: 0,
      left: - viewportWidth,
      behavior: "smooth",
    });

    setTimeout(()=>{
      if(moviesContainerEl.scrollLeft <= 0){
        console.log("SCROLL 0" + moviesContainerEl.scrollLeft);
        //leftBtn.style.display = "none";
      } else{
        //leftBtn.style.display = "flex";
      }
    }, 1000);
  }

</script>

<svelte:window on:resize={widthResized} />

<section>
  <Title text={title} />

  <div bind:this={moviesSlider}>

    <button bind:this={leftBtn} on:click={slideLeft} class="button-slider-left">
      <i class="fa-solid fa-angle-left" style="color: #ffffff;"></i>
    </button>

    <button bind:this={rightBtn} on:click={slideRight} class="button-slider-right">
      <i class="fa-solid fa-angle-right" style="color: #ffffff;"></i>
    </button>

    <div bind:this={moviesContainerEl}>
      {#each movies as movie}
        <Movie imageSrc={movie.img_src} />
      {/each}
    </div>
  </div>
</section>

<style>

  section{
    margin: 30px 0;
  }

  section > div{
    position: relative;
  }

  section > div > div{
    display: flex;
    gap: 10px;
    width: auto;
    overflow-x: scroll;
    overflow-y: hidden;
    padding-left: 60px;
  }

  section button{
    position: absolute;
    min-width: 40px;
    height: 100%;
    background-color: rgba(0, 0, 0, .6);
    display: flex;
    justify-content: center;
    align-items: center;
    cursor: pointer;
  }

  section button i{
    font-size: 30px;
  }

  section .button-slider-left{
    top: 0;
    left: 0;
  }

  section .button-slider-right{
    top: 0;
    right: 0;
  }

  @media screen and (max-width: 1200px){

  }

  @media screen and (max-width: 1024px){

  }

  @media screen and (max-width: 992px){
    section section button{
      display: none;
    }

    section > div > div{
      padding-left: 16px;
      padding: 0 16px;
    }
  }

</style>