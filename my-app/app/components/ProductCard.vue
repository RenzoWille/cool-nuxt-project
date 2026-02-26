<script setup>
const props = defineProps({
  product: {
    type: Object,
    default: () => ({
      id: 1,
      name: 'Viral content pack for your business',
      brand: 'GROWTH Social Media Agency',
      price: 650,
      originalPrice: 890,
      rating: 4.3,
      reviewCount: 128,
      image: 'https://images.unsplash.com/photo-1512941937669-90a1b58e7e9c?w=600&q=80',
      badge: 'Sale',
      inStock: true,
      tags: ['Reach millions', 'Grow online', 'Get more leads'],
    }),
  },
})

const added = ref(false)
const loading = ref(false)

async function addToCart() {
  if (loading.value || !props.product.inStock) return
  loading.value = true
  await new Promise(r => setTimeout(r, 600))
  added.value = true
  loading.value = false
  setTimeout(() => (added.value = false), 2000)
}

const discountPercent = computed(() => {
  if (!props.product.originalPrice) return null
  return Math.round((1 - props.product.price / props.product.originalPrice) * 100)
})
</script>

<template>
  <!--
    @container stelt de container query context in.
    Vereist: @tailwindcss/container-queries plugin in tailwind.config.
    Fallback: bloklay-out werkt altijd zonder JS/plugin.
  -->
  <article
    class="@container group relative bg-white rounded-2xl shadow-md hover:shadow-xl
           transition-shadow duration-300 overflow-hidden border border-stone-100"
    :aria-label="`Product: ${product.name}`"
  >
    <!-- Badge -->
    <div
      v-if="product.badge"
      aria-label="Productlabel"
      class="absolute top-3 left-3 z-10 bg-red-600 text-white text-xs
             font-bold px-2 py-1 rounded-full tracking-wide"
    >
      {{ product.badge }}
      <span v-if="discountPercent" class="ml-1">−{{ discountPercent }}%</span>
    </div>

    <!-- Uitverkocht overlay -->
    <div
      v-if="!product.inStock"
      aria-live="polite"
      class="absolute inset-0 z-20 bg-white/70 backdrop-blur-sm flex items-center justify-center"
    >
      <span class="text-stone-500 font-semibold text-sm tracking-widest uppercase">
        Uitverkocht
      </span>
    </div>

    <!--
      Container query layout:
      - Smal  (<20rem): gestapeld (flex-col)  — bijv. sidebar
      - Breed (>=20rem): naast elkaar (flex-row) — bijv. grid
    -->
    <div class="flex flex-col @[20rem]:flex-row">

      <!-- Afbeelding -->
      <div class="relative @[20rem]:w-2/5 @[20rem]:shrink-0 overflow-hidden bg-stone-50">
        <img
          :src="product.image"
          :alt="`Afbeelding van ${product.name}`"
          width="600"
          height="600"
          loading="lazy"
          decoding="async"
          class="w-full h-44 @[20rem]:h-full object-cover
                 transition-transform duration-500 group-hover:scale-105"
        />
      </div>

      <!-- Content -->
      <div class="flex flex-col justify-between p-4 @[20rem]:p-5 gap-3 flex-1">

        <!-- Merk + naam -->
        <div>
          <p class="text-xs text-stone-400 uppercase tracking-widest font-medium mb-0.5">
            {{ product.brand }}
          </p>
          <h2 class="text-base @[20rem]:text-lg font-bold text-stone-800 leading-snug">
            {{ product.name }}
          </h2>

          <!-- Tags -->
          <ul class="flex flex-wrap gap-1.5 mt-2" aria-label="Producteigenschappen">
            <li
              v-for="tag in product.tags"
              :key="tag"
              class="text-[10px] bg-stone-100 text-stone-500 px-2 py-0.5 rounded-full"
            >
              {{ tag }}
            </li>
          </ul>
        </div>

        <!-- Rating (semantisch: role="img" + aria-label) -->
        <div
          role="img"
          :aria-label="`Beoordeling: ${product.rating} van 5 sterren, ${product.reviewCount} reviews`"
          class="flex items-center gap-1.5"
        >
          <span class="flex text-amber-400 text-sm" aria-hidden="true">
            <template v-for="i in 5" :key="i">
              <span :class="i <= Math.round(product.rating) ? 'opacity-100' : 'opacity-20'">★</span>
            </template>
          </span>
          <span class="text-xs text-stone-400">({{ product.reviewCount }})</span>
        </div>

        <!-- Prijs + knop -->
        <div class="flex items-end justify-between gap-2 flex-wrap">
          <div>
            <span class="text-lg font-extrabold text-stone-900" aria-label="Huidige prijs">
              €{{ product.price.toFixed(2) }}
            </span>
            <span
              v-if="product.originalPrice"
              class="ml-1.5 text-sm text-stone-400 line-through"
              aria-label="Oorspronkelijke prijs"
            >
              €{{ product.originalPrice.toFixed(2) }}
            </span>
          </div>

          <!--
            Progressive enhancement:
            - Zonder JS: gebruik <form action="/cart" method="post"> als wrapper
            - Met JS: intercepten we click voor async feedback + optimistische UI
          -->
          <button
            type="button"
            :disabled="!product.inStock || loading"
            :aria-pressed="added"
            :aria-label="added ? 'Add to card' : 'Adding to cart'"
            class="relative inline-flex items-center gap-2 px-4 py-2 rounded-xl text-sm
                   font-semibold transition-all duration-200
                   focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2
                   focus-visible:outline-green-600
                   disabled:opacity-40 disabled:cursor-not-allowed"
            :class="added
              ? 'bg-green-500 text-white'
              : 'bg-stone-900 text-white hover:bg-green-400 active:scale-95'"
            @click="addToCart"
          >
            <!-- Loading spinner -->
            <svg
              v-if="loading"
              class="animate-spin h-4 w-4"
              aria-hidden="true"
              fill="none"
              viewBox="0 0 24 24"
            >
              <circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4"/>
              <path class="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8v8z"/>
            </svg>

            <!-- Vinkje bij succes -->
            <svg v-else-if="added" class="h-4 w-4" aria-hidden="true" fill="none"
                 stroke="currentColor" stroke-width="2.5" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" d="M5 13l4 4L19 7"/>
            </svg>

            <!-- Winkelwagen icoon -->
            <svg v-else class="h-4 w-4" aria-hidden="true" fill="none"
                 stroke="currentColor" stroke-width="2" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round"
                    d="M3 3h2l.4 2M7 13h10l4-10H5.4M7 13L5.4 5M7 13l-2 9m12-9l2 9
                       M9 21a1 1 0 100-2 1 1 0 000 2zm6 0a1 1 0 100-2 1 1 0 000 2z"/>
            </svg>

            <span>{{ added ? 'Added!' : 'In cart' }}</span>
          </button>
        </div>
      </div>
    </div>
  </article>
</template>