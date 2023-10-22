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

    buttonsSlides = [...content.querySelectorAll('div > section i')];

    try{
      const options = {
        method: 'GET',
        headers: {
          accept: 'application/json',
          Authorization: 'Bearer eyJhbGciOiJIUzI1NiJ9.eyJhdWQiOiIxM2FkNzIwZWM4NTY2NjNjNWI0ZjFkODk3MzI3OWQwMyIsInN1YiI6IjY1MmQwMGNkMWYzZTYwMDEzOTlmOGU1YiIsInNjb3BlcyI6WyJhcGlfcmVhZCJdLCJ2ZXJzaW9uIjoxfQ.mbSJcAq51x8-kXgJEdKY_1asnZc7RW4sBIl-B-N9qzU'
        }
      };
      const request = await fetch(fetchUrl, options);
      const response = await request.json();
      movies = response.results.slice(0, 8);
      
      for await(let movie of movies){
        let movieDetails;
        const movieURL = `https://api.themoviedb.org/3/movie/${movie.id}/credits?language=en-US`;

        const requestDetails = await fetch(movieURL, options);
        const responseDetails = await requestDetails.json();
        movieDetails = responseDetails.crew;

        if(requestDetails.ok){
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

        } else{
          console.log('Error picking movie');
        }

      }

    } catch (error){
      console.log('Error picking movies ', error);

    } finally{
      console.log("Ended fetch process.");
    }

    console.log('Test de listmovies: ', listMovies);

    slideEls = [...content.querySelectorAll('.movie')];
    slideEls = slideEls.slice(0, 8);
    slideEls[0].classList.add('active');


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

    e.stopPropagation();

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
    <button class="prev" on:click={prevSlide}><i class="fa-solid fa-angle-left" style="color: #ffffff;"></i></button>
    <button class="next" on:click={nextSlide}><i class="fa-solid fa-angle-right" style="color: #ffffff;"></i></button>
    {#each listMovies as movie, i}
      <div class="movie" data-id={i}>
        <aside class="gradient"></aside>
        <img src={`https://image.tmdb.org/t/p/w500${movie.backdrop_img}`} alt="">
        <div>
          <h2>{movie.title}</h2>
          <h3>{movie.director}</h3>
          <button>VER AHORA</button>
        </div>
      </div>
    {/each}

    <section on:click={selectSlide}>
      {#each listMovies as movie, i}
        <i data-id={i}></i>
      {/each}
    </section>
  </div>
</section>

<style>
  .wrapper{
    width: 100%;
    overflow-x: scroll;
    border: 2px solid black;
    position: relative;
    height: 600px;
    margin: 0 auto;
    display: flex;
  }

  .wrapper > button{
    background-color: unset;
    position: absolute;
    width: 50px;
    height: 100%;
    opacity: .3;
    cursor: pointer;
    z-index: 60;
    border: 0;
    transition: opacity .1s;
  }

  .wrapper > button:hover{
    opacity: 1;
  }

  .wrapper .prev{
    top: 0;
    left: 0;
  }

  .wrapper .next{
    top: 0;
    right: 0;
  }

  button i{
    font-size: 30px;
    color: rgba(255, 255, 255, .4);
  }

  button i:hover{
    color: rgba(255, 255, 255, 1);
  }

  .wrapper > div{
    flex-shrink: 0;
    width: 100%;
    height: 100%;
    position: absolute;
    opacity: 0;
    z-index: 0;
    display: flex;
    flex-direction: column;
    justify-content: flex-end;
    overflow: hidden;
  }

  .wrapper > div > .gradient{
    z-index: 5;
    width: 100%;
    height: 100%;
    position: absolute;
    background: linear-gradient(rgba(6, 6, 19, 0) 0%, rgba(6, 6, 19, .3) 60%, rgba(6, 6, 19, .4) 65%, rgba(6, 6, 19, .8) 85%, rgba(6, 6, 19, 1) 100%);
  }

  .wrapper > div > img{
    position: absolute;
    width: 100%;
    height: 100%;
    object-fit: cover;
  }

  .wrapper > div > div{
    margin-bottom: 50px;
    margin-left: 60px;
    position: relative;
    z-index: 6;
  }

  .wrapper > div > div > h2{
    font-size: 32px;
    font-weight: bold;
    color: white;
    max-width: 400px;
    margin-bottom: 20px;
  }

  .wrapper > div > div > h3{
    font-size: 24px;
    font-weight: bold;
    color: lightgray;
    max-width: 400px;
    margin-bottom: 20px;
  }

  .wrapper > div > div > button{
    height: auto;
    width: auto;
    padding: 8px 12px;
    border: 0;
    border-radius: 5px;
    color: black;
    font-size: 15px;
    font-weight: 600;
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

  @media screen and (max-width: 900px){
    .wrapper > div{
      align-items: center;

    }

    .wrapper > div > div{
      text-align: center;
      margin-left: 0;
      margin-bottom: 100px;
    }
  }



  /* GLOBAL STYLES */

  :global(.wrapper .active){
    z-index: 10 !important;
    opacity: 1 !important;
    transition: opacity 1s;
  }
</style>