<template>
    <!-- Fond et barre de navigation -->
    <div class="min-h-screen w-full bg-gradient-to-tr from-black via-stone-600 to-current pb-20">
        <nav class="sticky top-0 z-50 bg-black/75 backdrop-blur py-3">
            <div class="container mx-auto flex justify-center space-x-10">
                <NuxtLink to="/" class="text-lg text-white font-mono font-semibold hover:text-blue-700"
                    :class="{ 'text-blue-500': $route.path === '/' }">
                    Pokedex
                </NuxtLink>

                <NuxtLink to="/carte/" class="text-lg text-white font-mono font-semibold hover:text-blue-700"
                    :class="{ 'text-blue-500': $route.path === '/carte/' }">
                    Carte
                </NuxtLink>
            </div>
        </nav>
        <div class="container mx-auto">
            <slot />
        </div>
    </div>
</template>
