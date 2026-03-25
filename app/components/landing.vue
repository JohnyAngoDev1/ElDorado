<script setup lang="ts">
import { useTemplate } from '~/composables/useTemplate'

const { t } = useTemplate()
</script>

<template>
    <UPageSection class="bg-secondary" :ui="{ wrapper: 'py-8 sm:py-16' }">
        <UContainer>
            <div class="grid grid-cols-1 lg:grid-cols-2 gap-10 items-center">

                <div class="flex flex-col justify-between gap-10 py-4">

                    <div class="space-y-6">
                        <div class="space-y-2">
                            <h2 class="text-4xl lg:text-6xl font-black text-black tracking-tighter leading-[0.9]">
                                {{ t('landing_hero_title_main') }}
                            </h2>
                            <h2 class="text-4xl lg:text-6xl font-black text-primary tracking-tighter leading-[0.9]">
                                {{ t('landing_hero_title_highlight') }}
                            </h2>
                        </div>

                        <p class="text-gray-500 text-lg leading-relaxed max-w-lg">
                            {{ t('landing_hero_description') }}
                        </p>
                    </div>

                    <div class="flex flex-row justify-center gap-4">
                        <UButton :label="t('landing_secondary_button_text')" variant="outline"
                            class="w-full sm:w-auto justify-center rounded-xl px-8 py-4 text-black font-bold text-lg"
                            :to="`https://wa.me/${t('wspbutton_phone_number')}?text=${encodeURIComponent(t('wspbutton_generic_message'))}`" target="_blank" />
                    </div>

                    <div class="flex items-center gap-4 pt-4">
                        <div class="flex -space-x-3">
                            <div class="w-10 h-10 rounded-full bg-[#f1f5f9] border-2 border-white shadow-sm"></div>
                            <div class="w-10 h-10 rounded-full bg-[#F7F7F7] border-2 border-white shadow-sm"></div>
                            <div class="w-10 h-10 rounded-full bg-[#9E2C33] border-2 border-white shadow-sm"></div>
                        </div>
                        <p class="text-gray-500 text-sm font-medium">
                            {{ t('landing_trust_badge_text') }}
                        </p>
                    </div>

                </div>

                <NuxtImg :src="t('landing_hero_image_path')" class="w-full h-auto rounded-3xl shadow-2xl" />
            </div>
        </UContainer>
    </UPageSection>
</template>