<script setup lang="ts">
import CardsSrvCorp from '../components/cards.vue';
import { useTemplate } from '~/composables/useTemplate';

const { t } = useTemplate();
</script>

<template>
  <CardsSrvCorp hideHeader class="m-5" />
</template>
