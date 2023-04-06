<template>
  <div>
    <header class="shadow-sm bg-white">
      <nav class="container mx-auto p-4 flex justify-between">
        <NuxtLink to="/emailtemplate" class="font-bold">
          Email Template
        </NuxtLink>
        <NuxtLink to="/notes" class="font-bold">Notes</NuxtLink>
      </nav>
    </header>
    <div>
      <slot />
    </div>
  </div>
</template>
