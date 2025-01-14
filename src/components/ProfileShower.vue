<template>
 <form v-on:submit.prevent="fetchUserProfile" action="" class="md:w-1/2 px-3 mb-6 md:mb-0 " v-if="searchToggle">
  <div class="w-full">
    <label class="block uppercase tracking-wide text-grey-darker text-xs font-bold mb-2" for="grid-first-name">
  User Name
    </label>
    <input v-model="userName" 
      class="appearance-none block w-full bg-grey-lighter text-grey-darker border border-red rounded py-3 px-4 mb-3"
      id="grid-first-name" type="text" placeholder="Jane">
    <p class="text-red text-xs italic text-red" v-if="errorinput">Please fill out this field.</p>
  </div>
  <div class="text-right">
  <button type="submit" class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded">
    Button
  </button>
</div>
 </form>
  <div v-if="foundProfile"
    class="flex flex-col items-center justify-center h-full w-full bg-gray-400 rounded-md bg-clip-padding backdrop-filter backdrop-blur-sm bg-opacity-10 border border-gray-100 shadow-secondary-1 dark:bg-surface-dark md:w-1/2 px-3 mb-6 md:mb-0  m-10">
    <!-- Centering Image -->
    <div class="p-6 text-surface dark:text-white flex justify-center ">
      <img class="rounded-1/2 w-32 h-32 rounded-full"   :src="profile.avatar_url"
  alt="Profile Avatar"
/>
    </div>

    <!-- Other Content -->
    <div class="p-6 text-surface dark:text-white w-full">

      <div class="mb-4 text-base">
        <dl class="sm:divide-y sm:divide-gray-200">
            <div class="py-3 sm:py-5 sm:grid sm:grid-cols-3 sm:gap-4 sm:px-6">
                <dt class=" font-mono text-lg font-medium text-white">
                    Profile  name
                </dt>
                <dd class="mt-1 text-lg text-white sm:mt-0 sm:col-span-2 font-mono">
                  {{profile.name}}
                </dd>
            </div>
            <div class="py-3 sm:py-5 sm:grid sm:grid-cols-3 sm:gap-4 sm:px-6">
                <dt class="text-lg font-medium text-white">
                    Followers
                </dt>
                <dd class="mt-1 text-lg text-white sm:mt-0 sm:col-span-2 font-mono">
                  {{profile.followers}}
                </dd>
            </div>
            <div class="py-3 sm:py-5 sm:grid sm:grid-cols-3 sm:gap-4 sm:px-6">
                <dt class="text-lg font-medium text-white">
                   url
                </dt>
                <dd class="mt-1 text-lg text-white sm:mt-0 sm:col-span-2 w-auto font-mono">
                    <a :href=profile.url>{{profile.url  }}</a>
                </dd>
            </div>
           
        </dl>
      </div>
     
    </div>
  </div>
</template>
<script setup>
import { ref } from 'vue';
import axios from 'axios';

const profile = ref(null);
const spinner = ref(false)
const foundProfile = ref(false);
const errorinput=ref(false);
const searchToggle=ref(true);
const userName=ref('');
const fetchUserProfile = async () => {
  try {
  
    const response = await axios.get(`https://api.github.com/users/${userName.value}`);
    foundProfile.value=true;
    searchToggle.value=false;
    profile.value = response.data;
    console.log(profile.value);
    
  } catch (error) {
    if (error.response.status === 404) {
      errorinput.value=true;
        foundProfile.value = false;
      }
  }
 
};


</script>
<style lang="">

  </style>