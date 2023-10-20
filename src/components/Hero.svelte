<script>
  import {onMount} from 'svelte';

  export let fetchUrl;

  let fetchExample = [{}, {}, {}, {}, {}, {}, {}, {}];
  let content, slideEls;

  // let slides = [];
  let movies;
  let listMovies = [];
  let buttonsSlides = [];
  let currentPosition = 0;


  // ON LOAD

  onMount(async ()=>{
    console.log(content);

    slideEls = [...content.querySelectorAll('div')];
    slideEls[0].classList.add('active');

    buttonsSlides = [...content.querySelectorAll('div > section i')];
    console.log(slideEls);
    console.log(buttonsSlides);


    try{
      const options = {
        method: 'GET',
        headers: {
          accept: 'application/json',
          Authorization: 'Bearer eyJhbGciOiJIUzI1NiJ9.eyJhdWQiOiIxM2FkNzIwZWM4NTY2NjNjNWI0ZjFkODk3MzI3OWQwMyIsInN1YiI6IjY1MmQwMGNkMWYzZTYwMDEzOTlmOGU1YiIsInNjb3BlcyI6WyJhcGlfcmVhZCJdLCJ2ZXJzaW9uIjoxfQ.mbSJcAq51x8-kXgJEdKY_1asnZc7RW4sBIl-B-N9qzU'
        }
      };
      const request = await fetch('https://api.themoviedb.org/3/movie/top_rated?language=en-US&page=1', options);
      const response = await request.json();
      movies = response.results;
      
      movies.forEach(async (movie) =>{
        let movieDetails;

        try{
          const requestDetails = await fetch(`https://api.themoviedb.org/3/movie/${movie.id}/credits?language=en-US`, options);
          const responseDetails = await requestDetails.json();
          movieDetails = responseDetails.crew;

        } catch (error){
          console.log('Error picking movie ', error);
        } finally{

          const director = movieDetails.find(crewMember => {
            return crewMember.job === 'Director';
          });

          const item = {
            id: movie.id,
            poster_img: movie.poster_path,
            backdrop_img: movie.backdrop_path,
            title: movie.title,
            director: director.name,
            credits: movieDetails
          }

          listMovies = [...listMovies, item];
          console.log('Movie fetch', listMovies);
        }
  
      });
      console.log('Fetch PRINCIPAL peliculas', listMovies);
    } catch (error){
      console.log('Error picking movies ', error);

    } finally{
      console.log("Ended fetch process.");
      console.log('Fetch PRINCIPAL peliculas', listMovies);
    }
    
    setTimeout(()=>{
      console.log('onmount listmovies', listMovies);
    }, 2000);

  });


  // EVENTS

  function prevSlide(e){
    if(currentPosition === 0){
      currentPosition = slideEls.length-1;
      slideEls[0].classList.remove('active');
      slideEls[currentPosition].classList.add('active');
      return;
    }

    --currentPosition;
    slideEls[currentPosition + 1].classList.remove('active');
    slideEls[currentPosition].classList.add('active');
    console.log(`currentPosition: ${currentPosition}, slideEls.length: ${slideEls.length}`);
  }

  function nextSlide(e){

    if(currentPosition === slideEls.length - 1){
      currentPosition = 0;
      slideEls[slideEls.length-1].classList.remove('active');
      slideEls[currentPosition].classList.add('active');
      return;
    }

    ++currentPosition;
    slideEls[currentPosition - 1].classList.remove('active');
    slideEls[currentPosition].classList.add('active');
    console.log(`currentPosition: ${currentPosition}, slideEls.length: ${slideEls.length}`);

  }

  function selectSlide(e){
    e.preventDefault();
    e.stopPropagation();
    console.log(buttonsSlides);
    const data_id = parseInt(e.target.dataset.id);
    if(e.target.dataset.id){
      slideEls[currentPosition].classList.remove('active');
      currentPosition = data_id;
      slideEls[currentPosition].classList.add('active');
    }
  }

</script>

<section>

  <div class="wrapper" bind:this={content}>
    <button class="prev" on:click={prevSlide}></button>
    <button class="next" on:click={nextSlide}></button>
    {#each fetchExample as movie, i}
      <div data-id={i}>
        <img src="#" alt="">
        <h2>TÍTULO DE PELÍCULAS</h2>
        <h3>Nombre Autor</h3>
        <button>VER AHORA</button>
      </div>
    {/each}

    <section on:click={selectSlide}>
      {#each fetchExample as movie, i}
        <i data-id={i}></i>
      {/each}
    </section>
  </div>
</section>

<style>
  .wrapper{
    width: 90%;
    overflow-x: scroll;
    border: 2px solid black;
    position: relative;
    height: 300px;
    margin: 0 auto;
    display: flex;
  }

  .wrapper button{
    position: absolute;
    width: 50px;
    height: 100%;
    opacity: .3;
    cursor: pointer;
    z-index: 60;
  }

  .wrapper .prev{
    top: 0;
    left: 0;
  }

  .wrapper .next{
    top: 0;
    right: 0;
  }

  .wrapper div:nth-of-type(1){
    background-color: blue;
  }
  .wrapper div:nth-of-type(2){
    background-color: red;
  }

  .wrapper div:nth-of-type(3){
    background-color: yellow;
    /* transform: translateX(-100px); */
  }
  .wrapper div:nth-of-type(4){
    background-color: orange;
  }
  .wrapper div:nth-of-type(5){
    background-color: black;
  }
  .wrapper div:nth-of-type(6){
    background-color: white;
  }
  .wrapper div:nth-of-type(7){
    background-color: green;
  }
  .wrapper div:nth-of-type(8){
    background-color: lightgrey;
  }


  .wrapper div{
    flex-shrink: 0;
    width: 100%;
    height: 100%;
    position: absolute;
    opacity: 0;
    z-index: 0;
  }

  div > section{
    position: absolute;
    z-index: 30;
    width: 80%;
    height: 60px;
    left: 10%;
    bottom: 5%;
    display: flex;
    justify-content: center;
    align-items: center;
    gap: 30px;
  }

  div > section i{
    width: 15px;
    height: 15px;
    border-radius: 50%;
    background-color: rgba(255, 255, 255, .4);
    transform: scale(1);
    cursor: pointer;
    transition: background-color .1s, transform .1s;
  }

  div > section i:hover{
    background-color: rgba(255, 255, 255, 1);
    transform: scale(1.4);
  }



  /* GLOBAL STYLES */

  :global(.wrapper .active){
    z-index: 10 !important;
    opacity: 1 !important;
    transition: opacity 1s;
  }
</style>