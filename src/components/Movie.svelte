<script>
  export let imageSrc;
</script>

<div class="movie">
  <img src={imageSrc} />
</div>

<style>
  div.movie{
    width: 210px;
    height: 315px;
  }

  div > img{
    width: 100%;
    height: 100%;
  }
</style>