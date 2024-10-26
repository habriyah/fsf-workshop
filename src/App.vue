<script setup>
import {ref, onMounted} from 'vue';
import PocketBase from 'pocketbase';

const pb = new PocketBase('http://127.0.0.1:8090');
const titleVal = ref("");
const posts = ref([]);

async function Submit() {

  
const data = {
    Title: titleVal.value,
    description: "test"
};

const record = await pb.collection("posts").create(data);
posts.value.push(data);
}

onMounted(async () =>{
  
const data = await pb.collection('posts').getFullList();

posts.value = data;
})
</script>

<template>
  <input v-model="titleVal" class="bg-purple-400 ml-5 px-4 py-2 rounded-md transition "/>
  <button @click="Submit" class="bg-pink-200 text-white px-2 pt-5 pb-4 rounded-lg mx-1 my-2 transition duration-500 hover:bg-pink-500">Submit</button>

  <ul>
    <li v-for= "post in posts">
    <h3>{{ post.Title }}</h3>
  </li>
  </ul>
</template>