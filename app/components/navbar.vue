<script setup lang="ts">
import { useTemplate } from '~/composables/useTemplate'
import { useNavigation } from '~/composables/useNavigation'

const { t } = useTemplate()
const { navItems } = useNavigation()
</script>

<template>
    <UHeader>
        <template #left>
            <div class="flex items-center gap-3">
                <div v-if="t('navbar_brand_icon').startsWith('/') || t('navbar_brand_icon').includes('.')" class="flex items-center justify-center">
                    <NuxtImg :src="t('navbar_brand_icon')" class="h-10 w-auto object-contain" />
                </div>
                <div v-else class="p-1 rounded-lg flex items-center justify-center">
                    <UIcon :name="t('navbar_brand_icon')" class="text-2xl text-white" />
                </div>
                <span class="font-bold text-2xl text-black">
                    {{ t('navbar_brand_name') }}
                </span>
            </div>
        </template>

        <UNavigationMenu :items="navItems" variant="link" color="primary" class="hidden lg:flex" :ui="{
            link: 'text-black'
        }" />

        <template #right>
        </template>

        <template #body>
            <div class="p-4 space-y-6">
                <UNavigationMenu :items="navItems" orientation="vertical" color="primary" :ui="{
                    link: 'text-black'
                }" />
                <div class="flex items-center gap-2">
                </div>
            </div>
        </template>
    </UHeader>
</template>