<script>
  export let imageSrc;
</script>

<article>
  <img src={imageSrc} alt="example"/>
</article>

<style>
  article{
    width: 13.3%; /* 4 movies 23.7% */   /* 5 movies 18.7% */  
    height: auto;
    flex-shrink: 0;
    border-radius: 3%;
  }

  article > img{
    width: 100%;
    height: 100%;
    border-radius: 3%;
  }

  @media screen and (max-width: 1600px){
    article{
      width: 15.3%;
    }
  }

  @media screen and (max-width: 1200px){
    article{
      width: 18.7%;
    }
  }

  @media screen and (max-width: 1024px){
    
  }

  @media screen and (max-width: 992px){
    article{
      width: 23.7%;
    }
  }

  @media screen and (max-width: 845px){
    article {
      width: 31.7%;
    }
  }

</style>