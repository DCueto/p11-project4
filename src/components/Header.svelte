<script>
  import logo from '../assets/filmin_logo_w.png';


  // if(window.scrollY > 0){
  //   console.log(window.scrollY);
  // }
  
  // let header = document.querySelector('header');
  // console.log(header);

  // window.addEventListener('scroll', ()=>{
  //   if(window.scrollY > 0){
  //     header.classList.add('scrolledHeader');
  //     header.classList.remove('nonScrolledHeader');
  //   } else{
  //     header.classList.remove('scrolledHeader');
  //     header.classList.add('nonScrolledHeader');
  //   }
  // });

</script>

<header class="nonScrolledHeader">
  <div id="header_container">
    <div id="left_header">
      <img id="logo" src={logo} />
      <nav>
        <ul>
          <li><a href="#">Inicio</a></li>
          <li><a href="#">Películas</a></li>
          <li><a href="#">Series</a></li>
          <li><a href="#">Colecciones</a></li>
          <li><a href="#">Festivales</a></li>
          <li><a href="#">Más</a></li>
        </ul>
      </nav>
    </div>
    <div id="right_header">
      <button class="suscribe">SUSCRÍBETE</button>
      <i class="profile"></i>
      <i class="fa-solid fa-magnifying-glass search"></i>
    </div>
  </div>
</header>


<style>

  header{
    height: 156px;
    width: 100%;
    position: fixed;
  }

  header.nonScrolledHeader{
    background: linear-gradient(rgba(7, 7, 19, 1) 0%, rgba(7, 7, 19, 0.81) 20%, rgba(7, 7, 19, 0) 100%);
  }

  header.scrolledHeader{
    background: rgba(7, 7, 19, 1);
  }

  header #header_container{
    display: flex;
    justify-content: space-between;
    height: 67px;
    width: 100%;
    padding: 0 64px;
  }

  header #header_container #left_header{
    display: flex;
    justify-content: flex-start;
    height: 100%;
    align-items: center;
    gap: 60px;
  }

  header #header_container #right_header{
    display: flex;
    justify-content: flex-end;
    height: 100%;
    align-items: center;
    gap: 20px;
  }

  header img#logo{
    max-height: 25px;
  }

  nav > ul{
    display: flex;
    gap: 35px;
  }

  nav > ul > li{
    list-style: none;
    font-size: 17px;
    font-weight: 400;
  }

  li > a{
    text-decoration: none;
    color: white;
  }

  header button.suscribe{
    width: 108px;
    height: 33px;
    border: 0;
    border-radius: 5px;
    background-color: #24BB85;
    color: #25252F;
    font-weight: 600;
    font-size: 13px;
   }

  header i.profile{
    width: 42px;
    height: 38px;
    background-image: url('./../assets/face_icon@2x.png');
    background-size: 100%;
  }

  header i.search{
    width: 25px;
    height: 25px;
    color: white;
    font-size: 25px;
  }

  @media screen and (max-width: 1200px){
    nav > ul{
      gap: 15px;
    }
  }

  @media screen and (max-width: 1024px){
    nav{
      display: none;
    }
  }
</style>