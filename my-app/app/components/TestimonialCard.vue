<script setup>
const props = defineProps({
  review: {
    type: Object,
    default: () => ({
      id: 1,
      author: 'Renzo Wille',
      role: 'Eigenaar, GROWTH Webdesign',
      avatar: 'https://images.unsplash.com/photo-1519085360753-af0119f7cbe7?w=600&q=80',
      rating: 5,
      text: '"Sinds we het Viral Content Pack gebruiken, is onze online zichtbaarheid geëxplodeerd. We krijgen wekelijks nieuwe klanten via Instagram binnen. Het is de investering dubbel en dwars waard!"',
      date: '2 weken geleden'
    })
  }
})
</script>

<template>
  <article 
    class="@container bg-white p-5 rounded-2xl shadow-sm border border-stone-100 
           hover:shadow-md transition-shadow duration-300"
  >
    <div class="flex flex-col @[40rem]:flex-row gap-4 items-start">
      
      <img 
        :src="review.avatar" 
        alt="" 
        loading="lazy"
        width="150"
        height="150"
        class="w-12 h-12 @[40rem]:w-14 @[40rem]:h-14 rounded-full object-cover shrink-0 border-2 border-stone-50" 
      />
      
      <div class="flex-1 w-full">
        <div class="flex flex-col @[40rem]:flex-row @[40rem]:justify-between @[40rem]:items-start mb-2 gap-1">
          
          <div>
            <h3 class="font-bold text-stone-800 leading-tight">
              {{ review.author }}
            </h3>
            <p class="text-xs text-stone-500 mt-0.5">
              {{ review.role }}
            </p>
          </div>
          
          <span class="text-[10px] text-stone-400 font-medium uppercase tracking-wider">
            {{ review.date }}
          </span>
        </div>
        
        <div class="flex text-amber-400 text-sm mb-3">
          <template v-for="i in 5" :key="i">
            <span :class="i <= Math.round(review.rating) ? 'opacity-100' : 'opacity-20'">★</span>
          </template>
        </div>
        
        <p class="text-stone-600 text-sm leading-relaxed">
          {{ review.text }}
        </p>
      </div>
    </div>
  </article>
</template>