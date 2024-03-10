<template>
  <nav class="bg-pink-800 opacity-70 h-20"> <!-- Adjust the height here -->
    <div class="max-w-screen-xl mx-auto px-2 sm:px-6 lg:px-8 flex items-center justify-between h-full">
      <h1 class="text-4xl align-left font-extrabold tracking-tight text-white mr-4">
        Taskify
      </h1>
      <div class="hidden sm:block sm:ml-6">
        <div class="flex space-x-4">
          <nuxt-link to="/" :class="{ 'text-gray-100 hover:bg-pink-900 hover:text-white': true, 'px-3 py-2 rounded-md text-lg font-medium': true }">
            Home 
          </nuxt-link>
          <nuxt-link to="/about" :class="{ 'text-gray-100 hover:bg-pink-900 hover:text-white': true, 'px-3 py-2 rounded-md text-lg font-medium': true }">
            About
          </nuxt-link>
          <nuxt-link to="/contact" :class="{ 'text-gray-100 hover:bg-pink-900 hover:text-white': true, 'px-3 py-2 rounded-md text-lg font-medium': true }">
            Contact
          </nuxt-link>
        </div>
      </div>
    </div>
  </nav>
</template>
