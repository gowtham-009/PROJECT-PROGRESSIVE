<template>
    <div>
        <div class="grid grid-cols-1 p-2">
            <span class="text-gray-500 font-medium mt-1 text-md ml-1">Fetch a Particular Range of Data</span>
         <div class=" p-1 flex justify-start gap-2 w-full">
            <input type="date" v-model="startDate" class="px-4 py-2 border border-gray-300 rounded-md focus:outline-none focus:border-blue-500">
            <input type="date" v-model="endDate" class="px-4 py-2 border border-gray-300 rounded-md focus:outline-none focus:border-blue-500">
            <nuxt-link to="/"  class="bg-white p-2 mb-2 md:mb-0 bg-blue-600 rounded">
                <i class="fa-solid fa-rotate-left text-red-500"></i>
            </nuxt-link>
        </div>
      </div>
      <div class="container-m w-full p-2 bg-white flex">
        
          <div class="sub-container w-full px-5 py-2" style="background-color: #EFF4FA;">
              <div class="mini-container w-full p-2 bg-white">
                <div class="overflow-x-auto resizable-box" :style="{ height: boxHeight }">
  <table class="min-w-full divide-y divide-gray-200">
    <thead >
      <tr>
                  <th class="px-6 py-8 text-left text-md font-medium text-gray-600 uppercase tracking-widergap-1">
                    <button class="flex gap-2" @click="sortBy('id')">Customer ID
                    </button>
                    </th>
                    <th   class="px-6 py-3 text-left text-md font-medium text-gray-600 uppercase tracking-wider ">
                      <button class="flex gap-2" @click="sortBy('name')">User</button>
                    </th>
                    <th  class="px-6 py-3 text-left text-md font-medium text-gray-600 uppercase tracking-wider">
                      <button class="flex gap-2" @click="sortBy('date')">Joined</button>
                    </th>
                    <th class="px-6 py-3 text-left text-md font-medium text-gray-600 uppercase tracking-wider">
                      <button class="flex gap-2" @click="sortBy('id')">Status</button>
                    </th>
                    <th class="px-6 py-3 text-left text-md font-medium text-gray-600 uppercase tracking-wider">
                      <button class="flex gap-2" @click="sortBy('id')">Purchased</button>
                    </th>
                    <th class="px-6 py-3 text-left text-md font-medium text-gray-600 uppercase tracking-wider">
                      <button class="flex gap-2"  @click="sortBy('id')">Action</button>
                    </th>
                </tr>
    </thead>
    <tbody class="bg-white divide-y divide-gray-200">
      <tr v-for="(user, index) in paginatedUsers" :key="index" @click="showPersonDetails(user)">
        <td class="px-6 py-4 whitespace-nowrap"><h4 class="text-black-500 font-medium">#{{ user.id }}</h4></td>
        <td class="px-6 py-4 whitespace-nowrap"><h4 class="text-black-500 font-medium">{{ user.name }}</h4><p class="text-gray-400">{{ user.email }}</p></td>
        <td class="px-6 py-4 whitespace-nowrap"><h4 class="text-black-500 font-medium">{{ user.date }}</h4></td>
        <td class="px-6 py-4 whitespace-nowrap"><p class="p-2 bg-green-400 rounded-md text-white flex items-center justify-center">{{ user.status }}</p></td>
        <td class="px-6 py-4 whitespace-nowrap"><h4 class="text-black-500 font-medium">{{ user.purchased }}</h4></td>
        <td class="px-6 py-4 whitespace-nowrap flex gap-3 mt-3">
          <button type="button" class="m-1 ms-0 py-2 px-2 inline-flex items-center gap-x-2 text-sm font-semibold rounded-lg border border-transparent bg-red-600 text-white hover:bg-blue-700 disabled:opacity-50 disabled:pointer-events-none" data-hs-overlay="#hs-overlay-right"  @click="showPersonDetails(user)"><i class="fa-solid fa-eye text-xl text-white p-2"></i></button>
        </td>
      </tr>
    </tbody>
  </table>
</div>

 </div>
  </div>
  <div id="hs-overlay-right" class="hs-overlay hs-overlay-open:translate-x-0 hidden translate-x-full fixed top-0 end-0 transition-all duration-300 transform h-full max-w-xl w-full z-[80] bg-white border-s bg-white dark:border-gray-700" tabindex="-1">
        <div class="flex justify-between items-center py-3 px-4 border-b dark:border-gray-700">
          <h1 class="font-bold text-red-500 dark:text-red-500 "style="font-size: 2rem;">
            Customer Detail
          </h1>
          <button type="button" class="flex justify-center items-center size-7 text-sm font-semibold rounded-full border border-transparent text-black-500 hover:bg-gray-100 disabled:opacity-50 disabled:pointer-events-none dark:text-black dark:hover:bg-gray-700 dark:hover:text-white" data-hs-overlay="#hs-overlay-right">
            <span class="sr-only">Close modal</span>
            <svg class="flex-shrink-0 size-4" xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
              <path d="M18 6 6 18"></path>
              <path d="m6 6 12 12"></path>
            </svg>
          </button>
        </div>
        <div v-if="selectedPerson" class="p-3">
        <div class="grid grid-rows grid-flow-col-2 gap-2">
          <div class="p-2 ">
            <p class="text-black-300 text-xl" >ID:<h2 class="text-black-500 font-medium" style="font-size: 20px;"># {{ selectedPerson.id }}</h2></p>
          </div>
          <div class="p-2 ">
            <p class="text-black-300 text-xl" >Name:<h2 class="text-black-500 font-medium" style="font-size: 20px;"> {{ selectedPerson.name }}<label for="" class="text-red-500">*</label></h2></p>
          </div>
          
          <div class="p-2 ">
            <p class="text-black-300 text-xl">Email: <h2 class="text-black-500 font-medium" style="font-size: 20px;">{{ selectedPerson.email }}</h2></p>
          </div>
          <div class="p-2 ">
            <p class="text-black-300 text-xl">Joined Date: <h2 class="text-black-500 font-medium" style="font-size: 20px;">{{ selectedPerson.date }}</h2></p>
          </div>
          
          <div class="p-2 ">
            <p class="text-black-300 text-xl">Status: <h2 class="text-black-500 font-medium p-2 bg-green-400 rounded-md text-white flex items-center justify-center" style="font-size: 20px;">{{ selectedPerson.status }}</h2></p>
          </div>
          <div class="p-2 ">
            <p class="text-black-300 text-xl">Purchased: <h2 class=" font-medium text-white p-2 bg-blue-500 rounded-md  flex items-center justify-center" style="font-size: 20px;">{{ selectedPerson.purchased }}</h2></p>
          </div>
         
         
        </div>
</div>
     
    </div>
      </div>
      <!-- Pagination Controls -->
      <div class="pagination-controls flex justify-end gap-3 mt-4">
      <button @click="previousPage" :disabled="currentPage === 1" class="bg-blue-600 px-4 py-3 rounded text-white">
        Previous
      </button>
      <span class="mt-4">Pages {{ currentPage }} of {{ totalPages }}</span>
      <button @click="nextPage" :disabled="currentPage === totalPages" class="bg-red-600 px-4 py-3 rounded text-white">
        Next
      </button>
    </div>
    </div>
</template>

<script>
import usersdata from '~/data/user.js'; 
export default {
  data() {
    return {
        startDate: '',
      endDate: '',
      users:usersdata,
      currentPage: 1,
      pageSize: 10,
      sortColumn: '', 
      sortDirection: 1 ,
      selectedPerson: null,

    }
  },
  computed: {
    totalPages() {
    return Math.ceil(this.filteredUsers.length / this.pageSize);
  },
  paginatedUsers() {
    const startIndex = (this.currentPage - 1) * this.pageSize;
    const endIndex = startIndex + this.pageSize;
    return this.filteredUsers.slice(startIndex, endIndex);
  },
    filteredUsers() {
      if (!this.startDate || !this.endDate) {
        return this.users;
      }
      const startDate = new Date(this.startDate);
      const endDate = new Date(this.endDate);
      
      return this.users.filter(user => {
        const userDate = new Date(user.date);
        return userDate >= startDate && userDate <= endDate;
      });
    },
   
  },
  methods:{
    sortBy(column) {
      // If clicked on the same column, toggle sort direction
      if (this.sortColumn === column) {
        this.sortDirection *= -1;
      } else {
        // If clicked on a different column, set sort direction to ascending
        this.sortDirection = 1;
        this.sortColumn = column;
      }

      // Sort the users array based on the selected column and sort direction
      this.users.sort((a, b) => {
        if (a[column] < b[column]) {
          return -1 * this.sortDirection;
        }
        if (a[column] > b[column]) {
          return 1 * this.sortDirection;
        }
        return 0;
      });
    
      
    
    },
    nextPage() {
      if (this.currentPage < this.totalPages) {
        this.currentPage++;
      }
    },
    previousPage() {
      if (this.currentPage > 1) {
        this.currentPage--;
      }
    },
    showPersonDetails(user) {
      this.selectedPerson = user;
    },
    toggleDropdown() {
      this.isOpen = !this.isOpen;
    }
  }
}
</script>

<style>
@import url('~/assets/css/components.css');
</style>