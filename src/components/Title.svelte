<script>
  export let text;
</script>


<h1>{text}</h1>

<style>

  h1{
    color: white;
    font-size: 25px;
    letter-spacing: 1px;
    font-weight: 400;
    padding-left: 60px;
    padding-bottom: 30px;
  }

  @media screen and (max-width: 992px){
    h1{
      padding-left: 16px;
    }
  }

</style>