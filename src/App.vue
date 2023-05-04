<template>
  <nav>
    <div class="nav-center">
      <div class="logo">
        <img src="./assets/images/logo.png" alt=""> 
        <span>SESSIONS</span> 
      </div>

      <ul class="links">
        <li class="link">Info</li>
        <li class="link">Tasks</li>
        <li class="link">Speakers</li>
        <li class="link">Sponsors</li>
      </ul>

      <button class="btn">
        Register
      </button>
    </div>
  </nav>
  <ImageSlide />
</template>

<script>
import ImageSlide from './components/ImageSlide.vue';


export default {
  components: { ImageSlide }
  

}
</script>

<style scoped>
  nav {
    background: #fff;
    padding: 1rem;
  }

  .nav-center {
    display: flex;
    align-items: center;
    justify-content: space-between;
    width: 80%;
    margin: 0 auto;
  }

  .links {
    display: flex;
    align-items: center;
    justify-content: space-between;
    gap: 1.5rem;
    list-style-type: none;
  }

  .logo {
    display: flex;
    align-items: center;
  }
  .logo img {
    width: 60px;
    /* height: auto; */
  }
  .logo span {
    font-weight: 900;
  }

  .link, .logo {
    font-weight: bold;
    font-size: 1rem;
  }

  
 
</style>