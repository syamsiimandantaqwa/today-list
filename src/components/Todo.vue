<script setup> 
   defineProps({
      id: Number,
      title: String,
      desc: String,
      isDone: Boolean,
   })

</script>

<template>
 <div class="rounded-lg p-[15px] bg-cyan-200 w-[250px]">
    <h1 class="font-bold text-sm mb-[10px]">{{ title }}</h1>
    <p class="text-xs text-slate-600 break-words ">{{ desc }}</p>
    <div class="mt-[15px] flex gap-x-[10px] text-xs">
       <button @click="$emit('delete', id)" class="text-white bg-red-500 ring-1 rounded-lg p-2">hapus</button>
       <button v-show="!isDone" @click="$emit('done', id, title, desc)" class="text-white bg-cyan-500 ring-1 rounded-lg p-2">selesai</button>
    </div>
  </div>
</template>

<style scoped>
	

</style>