<script setup lang="ts">
import { ref, computed } from 'vue'
import { useTemplate } from '~/composables/useTemplate'
import { useServices } from '~/composables/useServices'
import type { Service } from '~/interfaces/service.interface'

const props = defineProps<{
  limit?: number
  hideHeader?: boolean
}>()

const { t } = useTemplate()
const { servicesList, pending, error } = useServices()

const selectedCategory = ref('Todos')

const categories = t('categories')

const filteredServices = computed(() => {
  if (!servicesList.value) return []
  let list = servicesList.value
  
  if (props.limit) {
    const result: Service[] = []
    const categoriesPool = categories.filter((c: string) => c !== 'Todos')
    
    // Pick first item found for each category
    for (const cat of categoriesPool) {
      if (result.length >= props.limit) break
      const item = list.find(s => s.category === cat)
      if (item) result.push(item)
    }
    
    // If we have fewer items than limit, fill with others
    if (result.length < props.limit) {
      const remaining = list.filter(s => !result.includes(s))
      result.push(...remaining.slice(0, props.limit - result.length))
    }
    
    return result
  }

  if (selectedCategory.value !== 'Todos') {
    list = list.filter(item => item.category === selectedCategory.value)
  }
  
  return list
})

const onclick = (nombre: string): void => {
  const phone = t('wspbutton_phone_number')
  const message = encodeURIComponent(`Hola, me interesa consultar el precio de: ${nombre}`)
  window.open(`https://wa.me/${phone}?text=${message}`, '_blank')
}
</script>

<template>
  <UPageSection :ui="{ wrapper: props.hideHeader ? 'py-0 sm:py-0' : 'py-8 sm:py-16', container: props.hideHeader ? 'py-0 sm:py-0' : 'py-8 sm:py-16' }">
    <UContainer>
      <div v-if="!props.hideHeader" class="flex flex-col items-center text-center mb-12">
        <span class="text-primary text-xs font-black uppercase tracking-widest block mb-4">
          {{ t('cards_section_subtitle') }}
        </span>

        <h2 class="text-4xl md:text-5xl font-black text-gray-900 tracking-tight mb-6">
          {{ t('cards_section_title') }}
        </h2>
        
      </div>

      <!-- Filtros de Categoría (Responsive: Scroll en móvil, Centrado en desktop) -->
      <div v-if="!props.limit" class="flex overflow-x-auto md:flex-wrap justify-start md:justify-center gap-2 mb-8 md:mb-12 pb-4 md:pb-0 no-scrollbar px-4 md:px-0">
        <button
          v-for="cat in categories"
          :key="cat"
          @click="selectedCategory = cat"
          :class="[
            'px-6 py-2 rounded-full text-sm font-bold transition-all duration-300 whitespace-nowrap',
            selectedCategory === cat
              ? 'bg-primary text-white shadow-lg shadow-primary/30 scale-105'
              : 'bg-secondary text-gray-600 hover:bg-gray-200'
          ]"
        >
          {{ cat }}
        </button>
      </div>

      <!-- Loading State -->
      <div v-if="pending" class="flex flex-wrap justify-center gap-8">
        <div v-for="n in (props.limit || 8)" :key="n" class="w-full md:w-[calc(50%-1.5rem)] lg:w-[calc(33.33%-2rem)] xl:w-[calc(25%-2rem)] max-w-[400px] h-[400px] bg-secondary animate-pulse rounded-4xl"></div>
      </div>

      <!-- Erros State -->
      <div v-else-if="error" class="text-center py-20">
        <p class="text-red-500 font-bold">{{ t('cards_section_error') }}</p>
      </div>

      <!-- Services Grid (CSS Grid for robust responsiveness) -->
      <div v-else class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-6 lg:gap-8 place-items-center">
        <UPageCard
          v-for="item in filteredServices"
          :key="item.title"
          class="w-full max-w-[400px] rounded-4xl bg-secondary group transition-all duration-500 hover:-translate-y-2 hover:shadow-2xl hover:shadow-primary/10 overflow-hidden flex flex-col h-full"
        >
          <template #header>
            <div class="relative overflow-hidden aspect-[3/4]">
              <NuxtImg
                :src="item.image"
                :alt="item.title"
                class="w-full h-full object-cover transition-transform duration-700 group-hover:scale-110"
                loading="lazy"
                placeholder
              />
              <div class="absolute top-4 left-4">
                <span class="bg-white/90 backdrop-blur-md px-3 py-1 rounded-full text-[10px] font-black uppercase tracking-wider text-primary shadow-sm">
                  {{ item.badge || item.category }}
                </span>
              </div>
            </div>
          </template>

          <div class="flex flex-col gap-2 mt-4">
            <h3 class="text-lg font-bold text-black leading-tight group-hover:text-primary transition-colors">
              {{ item.title }}
            </h3>
          </div>

          <div class="flex items-center justify-between border-t border-primary/10 pt-4 mt-auto">
            <UButton
              variant="link"
              color="primary"
              class="p-0 font-black text-sm tracking-tight hover:no-underline"
              @click="onclick(item.title)"
            >
              {{ t('cards_item_price_prefix') }}
            </UButton>
            <NuxtLink
              class="w-8 h-8 rounded-full bg-white flex items-center justify-center shadow-sm group-hover:bg-primary group-hover:text-white transition-all duration-300"
              @click="onclick(item.title)"
            >
              <UIcon name="material-symbols:arrow-forward" class="w-4 h-4" />
            </NuxtLink>
          </div>
        </UPageCard>
      </div>

      <!-- Botón Ver Todos (Solo si hay límite) -->
      <div v-if="props.limit && !pending && !error" class="flex justify-center mt-12">
        <UButton
          to="/servicios"
          size="xl"
          class="rounded-full px-10 py-4 font-black text-lg shadow-xl shadow-primary/20 hover:scale-105 transition-transform"
        >
          {{ t('cards_section_view_all') }}
          <template #trailing>
            <UIcon name="i-heroicons-arrow-right-20-solid" />
          </template>
        </UButton>
      </div>

      <!-- Empty State -->
      <div v-if="!pending && !error && filteredServices.length === 0" class="text-center py-20">
        <p class="text-gray-400">{{ t('cards_section_empty') }}</p>
      </div>
    </UContainer>
  </UPageSection>
</template>

<style scoped>
.rounded-4xl {
  border-radius: 2.5rem;
}
</style>