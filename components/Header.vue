<template>
    <div>
    <div class="main-container w-full p-2 bg-white">
     <div class="container w-full flex p-4">
        <div class="progrsss-box w-full p-1 bp-1">
            <H3 class="text-2xl text-black font-semibold">Project Progress Data</H3>
            <p class="text-lg font-medium text-gray-600">List of Recent Contracts and Freelancers</p>
        </div>
        <div class="progrsss-box w-full p-1 bp-2 flex items-center justify-end">
            <button class="bg-blue-700 p-2 text-white text-md rounded"><i class="fa-solid fa-arrow-up-from-bracket p-1"></i>Report</button>
        </div>
     </div>
     <nav class="w-full ml-5 mt-3 p-1 navbar">
       <ul class="navlinks w-full justify-start flex gap-5">
        <li> <nuxt-links  to="/" class="text-gray-400 text-lg font-medium navs">Progress</nuxt-links></li>
        <li><nuxt-links class="text-gray-400 text-lg font-medium navs" to="">Completed</nuxt-links></li>
        <li><nuxt-links class="text-gray-400 text-lg font-medium navs" to="">Invoice</nuxt-links></li>
       </ul>
     </nav>
    </div>
    </div>
</template>

<script>

</script>

<style>
@import url('~/assets/css/header.css');
</style>