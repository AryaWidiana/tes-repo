<script setup>
import { baseUrl, token } from '@/helper/GlobalVariable'
import axios from 'axios'
import { useRoute } from 'vue-router'

const prop = defineProps({
  title: String,
  content: String,
  blog_id: Number,
  slug: String
})

const route = useRoute().name

const deleteBlog = async () => {
  if (confirm('apakah anda yakin?')) {
    try {
      const response = await axios.delete(baseUrl + 'blog/' + prop.blog_id, {
        headers: {
          Authorization: `Bearer ${token}`
        }
      })
      if (response.status === 200) {
        location.reload()
      }
    } catch (error) {
      console.log(error)
    }
  }
}
</script>

<template>
  <div class="card-blog bg-white p-4">
    <h5 class="mt-5 text-xl font-semibold">
      {{ prop.title }}
    </h5>
    <p class="blog-content">
      {{ prop.content }}
    </p>
    <div class="mt-5 flex justify-center gap-5">
      <router-link
        :to="`/blog/update/` + prop.slug"
        v-show="route == 'author-blog'"
        class="btn btn-primary w-full text-center"
        >Update</router-link
      >
      <button v-show="route == 'author-blog'" class="btn btn-primary w-full" @click="deleteBlog">
        Delete
      </button>
    </div>
  </div>
</template>
