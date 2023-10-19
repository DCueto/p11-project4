<script>
  export let imageSrc;
</script>

<div class="movie">
  <img src={imageSrc} alt="example"/>
</div>

<style>
  div.movie{
    width: 13.3%; /* 4 movies 23.7% */   /* 5 movies 18.7% */  
    height: auto;
    flex-shrink: 0;
    border-radius: 3%;
  }

  div > img{
    width: 100%;
    height: 100%;
    border-radius: 3%;
  }

  @media screen and (max-width: 1600px){
    div.movie{
      width: 15.3%;
    }
  }

  @media screen and (max-width: 1200px){
    div.movie{
      width: 18.7%;
    }
  }

  @media screen and (max-width: 1024px){
    
  }

  @media screen and (max-width: 992px){
    div.movie{
      width: 23.7%;
    }
  }

  @media screen and (max-width: 845px){
    div.movie{
      width: 31.7%;
    }
  }

</style>