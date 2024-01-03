<template>
    <div class="min-h-screen w-full bg-cover bg-no-repeat" :style="{ backgroundImage: 'url(/background.png)' }">
        <!-- Barre de navigation centrée et sticky -->
        <nav class="sticky top-0 z-50 bg-black/75 backdrop-blur py-3">
            <div class="container mx-auto flex justify-center space-x-10">
                <!-- Lien Pokedex -->
                <NuxtLink to="/" class="text-sm text-white font-mono font-semibold hover:text-blue-700"
                    :class="{ 'text-blue-500': $route.path === '/' }">
                    Pokedex
                </NuxtLink>

                <!-- Lien Carte -->
                <NuxtLink to="/carte/" class="text-sm text-white font-mono font-semibold hover:text-blue-700"
                    :class="{ 'text-blue-500': $route.path === '/carte/' }">
                    Carte
                </NuxtLink>
            </div>
        </nav>
        <div class="container mx-auto px-3">
            <slot />
        </div>
    </div>
</template>
