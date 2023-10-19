<script>
  import {onMount} from 'svelte';

  export let fetchUrl;

  let content, slideEls;

  let slides = [];
  let currentPosition = 0;


  // ON LOAD

  onMount(()=>{
    console.log(content);
    slideEls = [...content.querySelectorAll('div')];
    console.log(slideEls);
    
    slides = slideEls.map((el) => {
      const slide = {
        id: el.dataset.id,
        node: el,
        active: false,
      }
      return slide;
    });

    slides[0].active = true;

    console.log(slides);
    slideEls[0].classList.add('active');

    // slides.forEach((el, index, array) =>{
    //   if(el.active == true){
    //     slideEls[index].style.zIndex = 10;
    //     slideEls[index].style.opacity = 1;
    //   }
    // });

  });


  // EVENTS

  function prevSlide(e){
    --currentPosition;
    console.log(`currentPosition: ${currentPosition}, slideEls.length: ${slideEls.length}`);
    slideEls[currentPosition + 1].classList.remove('active');
    slideEls[currentPosition].classList.add('active');
  }

  function nextSlide(e){
    ++currentPosition;
    if(currentPosition === slideEls.length){
      currentPosition = 0;
      slideEls[slideEls.length-1].classList.remove('active');
      slideEls[currentPosition].classList.add('active');
    }
    console.log(`currentPosition: ${currentPosition}, slideEls.length: ${slideEls.length}`);
    slideEls[currentPosition - 1].classList.remove('active');
    slideEls[currentPosition].classList.add('active');
    // if(currentPosition === slideEls.length -1){
    //   currentPosition = 0;
    //   slideEls[slideEls.length-1].classList.remove('active');
    //   slideEls[currentPosition].classList.add('active');
    // }
  }

</script>

<section>

  <div class="wrapper" bind:this={content}>
    <button class="prev" on:click={prevSlide}></button>
    <button class="next" on:click={nextSlide}></button>
    <div data-id="1"></div>
    <div data-id="2"></div>
    <div data-id="3"></div>
    <div data-id="4"></div>
    <div data-id="5"></div>
    <div data-id="6"></div>
    <div data-id="7"></div>
    <div data-id="8"></div>
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


  /* GLOBAL STYLES */

  :global(.wrapper .active){
    z-index: 10 !important;
    opacity: 1 !important;
    transition: opacity 1s;
  }
</style>