<template>
  <div>
    <div class="text-body-1 text-md-subtitle-1">
      <h1 class="text-h4 text-md-h2 font-weight-bold text-center">{{ title }}</h1>
      <p class="mt-10">
        {{ description }}
      </p>
      <p><span class="font-weight-bold">Important:</span> {{ disclaimer }}</p>
    </div>
  </div>
</template>

<script>
export default {
  name: 'TitleBanner',
  props: {
    title: { type: String, default: '' },
    description: { type: String, default: '' },
    disclaimer: { type: String, default: '' },
  },
  data: () => ({
    //
  }),
}
</script>
