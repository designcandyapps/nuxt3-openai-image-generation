<script setup lang="ts">
const { data:page }=await useAsyncData('index',()=>queryContent('/').findOne())
useSeoMeta({
  titleTemplate:'',
  title:page.value.title,
  ogTitle:page.value.title,
  description:page.value.description,
  ogDescription:page.value.description
})
const apiKey=ref('')
const prompt=ref('')
const size=ref('large')
const imageSize=computed(()=>{
  switch(size.value){
    case 'large':return 1024
    case 'medium':return 512
    case 'small':return 256
    default:return 0
  }
})
const imageUrl=ref('')
const formErrorMessage=ref('')
const loading=ref(false)
async function generateImage(){
  formErrorMessage.value=''
  loading.value=true
  const {data,error}=await useFetch('/api/openai-generate-image',{
    method:'post',
    body:{
      apiKey:apiKey.value,
      prompt:prompt.value,
      size:size.value
    }
  })
  imageUrl.value=data.value??''



  if(error.value){formErrorMessage.value=error.value.data.data.error.message}
  loading.value=false
}
const toast=useToast()
function onClick(){
  alert('Click!')
}
</script>

<template>
  <div>
    <ULandingHero v-if="page.hero" v-bind="page.hero">
      <template #headline>
        <UBadge v-if="page.hero.headline" variant="subtle" size="lg" class="relative rounded-full font-semibold">
          <NuxtLink :to="page.hero.headline.to" class="focus:outline-none"><span class="absolute inset-0" aria-hidden="true"/></NuxtLink>
          {{page.hero.headline.label}}<UIcon v-if="page.hero.headline.icon" :name="page.hero.headline.icon" class="ml-1 w-4 h-4 pointer-events-none" />
        </UBadge>
      </template>
      <template #title><MDC :value="page.hero.title" /></template>


<template>
<div class="max-w-5xl py-8 px-4 mx-auto">
<div class="mt-8 p-4 grid grid-cols-1 gap-6 items-start border rounded">
<form class="grid grid-cols-1 gap-6" @submit.prevent="generateImage">
<input type="text" v-model="apiKey" class="form-input rounded mt-1 block w-full dark:bg-nuxt-gray" placeholder="API Key">
<input type="text" v-model="prompt" class="form-input rounded mt-1 block w-full dark:bg-nuxt-gray" placeholder="Description">
<select id="size" v-model="size" class="form-select rounded mt-1 block w-full dark:bg-nuxt-gray"><option value="large">Large</option></select>
<button type="submit" class="inline-flex justify-center items-center rounded border border-transparent bg-nuxt-green py-2 px-4 text-sm font-medium text-white shadow-sm hover:bg-nuxt-green focus:outline-none focus:ring-2 focus:ring-nuxt-green focus:ring-offset-2" :class="{'animate-spi':loading}" :disabled="loading">Generate</button><span v-if="formErrorMessage" class="text-red-500">{{formErrorMessage}}</span></form><p v-if="loading">Gen...</p>
<nuxt-img v-else-if="!loading&&imageUrl" :src="imageUrl" :width="imageSize" :height="imageSize" />
</div></div>
<UButton label="Show" @click="toast.add({title:'Click',click:onClick})" />
</template>


      <MDC :value="page.hero.code" class="prose prose-primary dark:prose-invert mx-auto" />
    </ULandingHero>
    <ULandingSection :title="page.features.title" :links="page.features.links">
      <UPageGrid><ULandingCard v-for="(item,index) of page.features.items" :key="index" v-bind="item" /></UPageGrid>
    </ULandingSection>
  </div>
</template>
