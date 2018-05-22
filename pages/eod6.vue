<template>
      <LandingPageComponent :onFormSubmitUrl="onFormSubmitUrl" :pagetypeInfo="pagetypeInfo"/>
</template>

<script>
import LandingPageComponent from '@/components/landing';

export default {
  components: { LandingPageComponent },
  async asyncData({ app }) {
    const response = await app.$axios.$post(
      'https://runflow.built.io/run/27mqbwl0xi?sync=true',
      { pageUrl: 'eod6' }
    );
    return { pagetypeInfo: response.pagetypeInfo };
  },
  data() {
    return {
      onFormSubmitUrl: 'https://runflow.built.io/run/1DGm8LpMUw?sync=true',
    };
  },
};
</script>
