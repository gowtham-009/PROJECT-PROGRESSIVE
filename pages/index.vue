 <template>
  <div class=" w-full">
     <div class="grid grid-cols p-2">
      <div class=" p-1 flex justify-between w-full">
        <div class="mb-1 flex gap-5 justify-start items-center pn">
          <label for="checkbox" class="text-gray-500 text-lg font-medium">Information</label>
        </div>
        <div class="mb-1 pn">
          <p class="text-gray-500 text-lg font-medium px-4 py-3">Project Name</p>
        </div>
        <div class="mb-1 flex md:flex-row md:items-center gap-2">
          <div class="relative">
            <input type="text" class="w-full md:w-auto px-4 py-3 focus:outline-none focus:border-blue-500"
              v-model="searchQuery" placeholder="Search user name...">
          </div>
          
          <div class="relative inline-block text-left">
    <div>
      <button @click="toggleDropdown" type="button" class="inline-flex justify-center w-full rounded-md border border-gray-300 shadow-sm px-5 py-3 bg-white text-sm font-medium text-gray-700 hover:bg-gray-50 focus:outline-none focus:ring-2 focus:ring-indigo-500 focus:ring-offset-2 focus:ring-offset-gray-100">
    Hide/Show Column
    <svg v-if="isOpen" class="-mr-1 ml-2 h-5 w-5" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20" fill="currentColor" aria-hidden="true">
        <path fill-rule="evenodd" d="M5.293 7.293a1 1 0 011.414 0L10 10.586l3.293-3.293a1 1 0 111.414 1.414l-4 4a1 1 0 01-1.414 0l-4-4a1 1 0 010-1.414z" clip-rule="evenodd" />
    </svg>
    <svg v-else class="-mr-1 ml-2 h-5 w-5" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20" fill="currentColor" aria-hidden="true">
        <path fill-rule="evenodd" d="M5.293 7.293a1 1 0 011.414 0L10 10.586l3.293-3.293a1 1 0 111.414 1.414l-4 4a1 1 0 01-1.414 0l-4-4a1 1 0 010-1.414z" clip-rule="evenodd" />
    </svg>
</button>

    </div>

    <div v-if="isOpen" class="origin-top-right absolute right-0 mt-2 w-56 rounded-md shadow-lg bg-white ring-1 ring-black ring-opacity-5">
      <div class="py-1 drop-menu">
          
          <label class="text-md flex gap-2" for="id">
            <input class="cols" type="checkbox" id="id" v-model="columnVisibility.id" >Customer ID
          </label>
          <label class="text-md flex gap-2" for="name">
            <input class="cols" type="checkbox" id="name" v-model="columnVisibility.name">User
          </label>
          <label class="text-md flex gap-2" for="date">
            <input class="cols" type="checkbox" id="date" v-model="columnVisibility.date">Joined
          </label>
          <label class="text-md flex gap-2" for="status">
            <input class="cols" type="checkbox" id="status" v-model="columnVisibility.status">Status
          </label>
          <label class="text-md flex gap-2" for="purchased">
            <input class="cols" type="checkbox" id="purchased" v-model="columnVisibility.purchased">Purchased
          </label>
          <label class="text-md flex gap-2" for="action">
            <input class="cols" type="checkbox" id="action" v-model="columnVisibility.action">Action

          </label>
        </div>
    </div>
  </div>


          <nuxt-link to="/datetable" class="bg-white p-2 mb-2 md:mb-0">
            <i class="fa-solid fa-table text-gray-400"></i>
          </nuxt-link>
          <button class="bg-blue-700 p-2 text-white text-md rounded mb-2 md:mb-0 md:mr-2">
            <i class="fa-solid fa-file p-1"></i>
            <span class="hidden md:inline">Team files</span>
          </button>
          <button @click="exportDataToCsv" class="bg-red-700 p-2 text-white text-md rounded mb-2 md:mb-0 md:mr-2">
            <i class="fa-solid fa-file-export p-1"></i>
            <span class="hidden md:inline">Export CSV</span>
          </button>
        </div>
      </div>
    </div>
    <div class="container-m w-full p-1 bg-white">

      <div class="sub-container w-full px-3 py-2" style="background-color: #EFF4FA;">
        <div class="mini-container w-full p-1 bg-white">
          <div class="overflow-x-auto resizable-box">
            <table class="min-w-full divide-y divide-gray-200 box" >
              <thead>
                <tr>
                  <th v-if="columnVisibility.id" class="px-6 py-8 text-left text-md font-medium text-gray-600 uppercase tracking-widergap-1">
                    <button class="flex gap-2" @click="sortBy('id')">Customer ID
                    </button>
                    </th>
                    <th v-if="columnVisibility.name"  class="px-6 py-3 text-left text-md font-medium text-gray-600 uppercase tracking-wider ">
                      <button class="flex gap-2" @click="sortBy('name')">User</button>
                    </th>
                    <th v-if="columnVisibility.date" class="px-6 py-3 text-left text-md font-medium text-gray-600 uppercase tracking-wider">
                      <button class="flex gap-2" @click="sortBy('date')">Joined</button>
                    </th>
                    <th v-if="columnVisibility.status" class="px-6 py-3 text-left text-md font-medium text-gray-600 uppercase tracking-wider">
                      <button class="flex gap-2" @click="sortBy('id')">Status</button>
                    </th>
                    <th  v-if="columnVisibility.purchased" class="px-6 py-3 text-left text-md font-medium text-gray-600 uppercase tracking-wider">
                      <button class="flex gap-2" @click="sortBy('id')">Purchased</button>
                    </th>
                    <th v-if="columnVisibility.action" class="px-6 py-3 text-left text-md font-medium text-gray-600 uppercase tracking-wider">
                      <button class="flex gap-2"  @click="sortBy('id')">Action</button>
                    </th>
                </tr>
              </thead>
              <tbody class="bg-white divide-y divide-gray-200">
                <tr v-for="(user, index) in paginatedUsers" :key="index" @click="showPersonDetails(user)">
                  <td v-if="columnVisibility.id" class="px-6 py-4 whitespace-nowrap">
                    <h4 class="text-black-500 font-medium">#{{ user.id }}</h4>
                  </td>
                  <td v-if="columnVisibility.name" class="px-6 py-4 whitespace-nowrap">
                    <h4 class="text-black-500 font-medium">{{ user.name }}</h4>
                    <p class="text-gray-400">{{ user.email }}</p>
                  </td>
                  <td v-if="columnVisibility.date" class="px-6 py-4 whitespace-nowrap">
                    <h4 class="text-black-500 font-medium">{{ user.date }}</h4>
                  </td>
                  <td v-if="columnVisibility.status" class="px-6 py-4 whitespace-nowrap">
                    <p class="p-2 bg-green-400 rounded-md text-white flex items-center justify-center">{{ user.status }}
                    </p>
                  </td>
                  <td v-if="columnVisibility.purchased" class="px-6 py-4 whitespace-nowrap">
                    <h4 class="text-black-500 font-medium">{{ user.purchased }}</h4>
                  </td>
                  <td v-if="columnVisibility.action" class="px-6 py-4 whitespace-nowrap flex gap-3 mt-3">
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
import users from '~/data/user.js';
export default {
  data() {
    return {
      users: users,
      currentPage: 1,
      pageSize: 10,
      sortColumn: '', 
      sortDirection: 1 ,
      searchQuery:'',
      selectedPerson: null,
      isOpen: false,
      columnVisibility: {
        id: true,
        name: true,
        date: true,
        status: true,
        purchased: true,
        action: true
      },
     
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
    return this.users.filter(user => {
      // Filter by user id or name
      return user.id.toString().includes(this.searchQuery.toLowerCase()) ||
             user.name.toLowerCase().includes(this.searchQuery.toLowerCase()) ||
             user.email.toLowerCase().includes(this.searchQuery.toLowerCase());
    });
  }
},
  methods: {
    exportDataToCsv() {
   const csv=users;
      const csvContent = this.convertToCsv(csv);
      const blob = new Blob([csvContent], { type: 'text/csv;charset=utf-8' });
      const url = URL.createObjectURL(blob);
      const link = document.createElement('a');
      link.href = url;
      link.setAttribute('download', 'export_data.csv');
      link.click();
    },
    convertToCsv(data) {
      const headers = Object.keys(data[0]);
      const rows = data.map(obj => headers.map(header => obj[header]));
      const headerRow = headers.join(',');
      const csvRows = [headerRow, ...rows.map(row => row.join(','))];
      return csvRows.join('\n');
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
    
    showPersonDetails(user) {
      this.selectedPerson = user;
    },
    toggleDropdown() {
      this.isOpen = !this.isOpen;
    }
  },
}


</script>

<style>
@import url('~/assets/css/components.css');

.drop-menu{
  display: flex;
  flex-direction: column;
  justify-content: space-evenly;
  padding: 5%;
}
</style> 