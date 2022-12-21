<template>
  <header class="bg-indigo-600">
    <div class="container px-5 mx-auto h-20 flex justify-between items-center text-white">
      <NuxtLink :to="{ name: 'index' }" class="inline-flex items-center gap-x-5">
        <SvgIcon name="icons/logo" class="w-10 h-10" />
        Task editor
      </NuxtLink>
      <button class="py-2 px-4 transition-colors duration-100 ease-in-out rounded-md bg-white text-indigo-600 hover:bg-indigo-600 hover:text-white">
        Add
      </button>
    </div>
  </header>
</template>
