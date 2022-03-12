<template>
  <svg
    v-if="showIcon === 'trash'"
    width="24"
    height="24"
    stroke-width="1.5"
    viewBox="0 0 24 24"
    fill="none"
    xmlns="http://www.w3.org/2000/svg">
    <path
      d="M19 11V20.4C19 20.7314 18.7314 21 18.4 21H5.6C5.26863 21 5 20.7314 5 20.4V11"
      stroke="currentColor"
      stroke-linecap="round"
      stroke-linejoin="round" />
    <path
      d="M10 17V11"
      stroke="currentColor"
      stroke-linecap="round"
      stroke-linejoin="round" />
    <path
      d="M14 17V11"
      stroke="currentColor"
      stroke-linecap="round"
      stroke-linejoin="round" />
    <path
      d="M21 7L16 7M3 7L8 7M8 7V3.6C8 3.26863 8.26863 3 8.6 3L15.4 3C15.7314 3 16 3.26863 16 3.6V7M8 7L16 7"
      stroke="currentColor"
      stroke-linecap="round"
      stroke-linejoin="round" />
  </svg>

  <svg
    v-if="showIcon === 'add'"
    width="24"
    height="24"
    stroke-width="1.5"
    viewBox="0 0 24 24"
    fill="none"
    xmlns="http://www.w3.org/2000/svg">
    <path
      d="M8 12H12M16 12H12M12 12V8M12 12V16"
      stroke="currentColor"
      stroke-linecap="round"
      stroke-linejoin="round" />
    <path
      d="M12 22C17.5228 22 22 17.5228 22 12C22 6.47715 17.5228 2 12 2C6.47715 2 2 6.47715 2 12C2 17.5228 6.47715 22 12 22Z"
      stroke="currentColor"
      stroke-linecap="round"
      stroke-linejoin="round" />
  </svg>

  <svg
    v-if="showIcon === 'cancel'"
    width="24"
    height="24"
    stroke-width="1.5"
    viewBox="0 0 24 24"
    fill="none"
    xmlns="http://www.w3.org/2000/svg">
    <path
      d="M6.75827 17.2426L12.0009 12M17.2435 6.75736L12.0009 12M12.0009 12L6.75827 6.75736M12.0009 12L17.2435 17.2426"
      stroke="currentColor"
      stroke-linecap="round"
      stroke-linejoin="round" />
  </svg>

  <svg
    v-if="showIcon === 'check'"
    width="24"
    height="24"
    stroke-width="1.5"
    viewBox="0 0 24 24"
    fill="none"
    xmlns="http://www.w3.org/2000/svg">
    <path
      d="M1.5 12.5L5.57574 16.5757C5.81005 16.8101 6.18995 16.8101 6.42426 16.5757L9 14"
      stroke="currentColor"
      stroke-linecap="round" />
    <path d="M16 7L12 11" stroke="currentColor" stroke-linecap="round" />
    <path
      d="M7 12L11.5757 16.5757C11.8101 16.8101 12.1899 16.8101 12.4243 16.5757L22 7"
      stroke="currentColor"
      stroke-linecap="round" />
  </svg>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
export default defineComponent({
  name: 'Icons',
  props: {
    showIcon: {
      type: String,
      required: true,
    },
  },
});
</script>
