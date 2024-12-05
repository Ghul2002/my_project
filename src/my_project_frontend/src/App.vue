<script setup>
import { ref } from 'vue';
import { my_project_backend } from 'declarations/my_project_backend/index';
let blogs = ref([]);

async function handleSubmit(e) {
  e.preventDefault();
  const target = e.target;
  const title = target.querySelector('#title').value;
  const content = target.querySelector('#content').value;
  const tags = target.querySelector('#tags').value;

  const splittedTags = tags.split(",")

  await my_project_backend.add_blog(title, content, splittedTags)
  await getBlogs()
}

async function getBlogs() {
  const tempBlogs = await my_project_backend.get_blogs()
  blogs.value = tempBlogs.map((blog) => {
    return {
      ...blog,
      date: blog.date.toString()
    }
  })
}
getBlogs()
</script>

<template>
  <main class="container mx-auto">
    <img src="/logo2.svg" alt="DFINITY logo" class="mx-auto mt-4"/>
    <br />
    <br />
    <form class="grid gap-4 pb-4 mb-4 border-sold border-b-2 border-sky-500" action="#" @submit="handleSubmit">
      <div>
        <p class="text-white"> Title: </p>
        <input 
        id="title" 
        alt="title" 
        type="text" 
        class="w-full rounded-full py-1 px-4 outline-none border-solid border-2 hover:border-emerald-700"/>
      </div>
      <div>
        <p class="text-white">Content: </p>
        <textarea
        id="content" 
        alt="content" 
        type="text" 
        class="w-full rounded-3xl py-1 px-4 outline-none min-h-[100p] border-solid border-2 hover:border-emerald-700">
        </textarea>
      </div>
      <div>
        <p class="text-white">Tags: </p>
        <input 
        id="tags" 
        alt="tags" 
        type="text"
        class="w-full rounded-full py-1 px-4 outline-none border-solid border-2 hover:border-emerald-700" />
      </div>
      <div class="flex justify-end">
        <button 
        type="submit" 
        class="text-white bg-emerald-700 rounded-full py-1 px-4 outline-none ">
        Click to add!
      </button>
      </div>

    </form>
    <div>
      <div v-for="blog in blogs">
        <h3>{{ blog.title }}</h3>
        <p>{{ blog.content }}</p>
        <div>
          {{ blog.date }}
          {{ blog.tags }}
        </div>
      </div>
    </div>
  </main>
</template>
