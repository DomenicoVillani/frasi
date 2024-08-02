<script setup>
const annoAttuale = ref(new Date().getFullYear());
</script>

<template>
  <div class="wrapper">
    <header class="header p-4">
      <div class="navbar flex  justify-content-center">
        <NuxtLink to="/" class="no-underline text-0 mr-5">Home</NuxtLink>
        <NuxtLink to="/barzellette" class="no-underline text-0 mr-5">Barzellette</NuxtLink>
        <NuxtLink to="/frasi" class="no-underline text-0 mr-5">Frasi</NuxtLink>
      </div>
    </header>
    
    <main class="content">
      <NuxtPage />
    </main>

    <footer class="footer">
      <div class="container mx-auto flex justify-content-evenly">
        <div>&copy; <span>{{ annoAttuale }}</span> Domenico Villani.</div>
        <div class="flex">
                <a href="mailto:domenico.villani.99@gmail.com" class="text-0"><i class="pi pi-envelope text-2xl mr-2"></i></a>
                <a href="tel:+393203571291" class="text-0"><i class="pi pi-phone text-2xl mr-2"></i></a>
                <a href="https://github.com/DomenicoVillani" target="_blank" class="text-0"><i class="pi pi-github text-2xl mr-2"></i></a>
                <a href="https://www.linkedin.com/in/domenicovillani99/" target="_blank" class="text-0"><i class="pi pi-linkedin text-2xl mr-2"></i></a>
                <a href="https://www.instagram.com/domenico_villani_/" target="_blank" class="text-0"><i class="pi pi-instagram text-2xl mr-2"></i></a>
                <a href="https://t.me/Domm99" target="_blank" class="text-0"><i class="pi pi-telegram text-2xl mr-2"></i></a>
            </div>
      </div>
    </footer>
  </div>
</template>


<style>

body, html {
  margin: 0;
  padding: 0;
  height: 100%;
  box-sizing: border-box;
}

.wrapper {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
  background-color: #8856dd;
}

.header {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  color: white;
  padding: 16px;
  background-color: #7C3AED;
  z-index: 10;
}

.content {
  margin-top: 80px;
  margin-bottom: 50px;
  padding: 16px;
  flex: 1;
}

.footer {
  position: fixed;
  bottom: 0;
  left: 0;
  width: 100%;
  color: white;
  padding: 16px;
  text-align: center;
  background-color: #7C3AED;
}
</style>