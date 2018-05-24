<template>
      <LandingPageComponent
        :onFormSubmitUrl="onFormSubmitUrl"
        :pagetypeInfo="pagetypeInfo"
        :pageUrl="url"
      />
</template>

<script>
import LandingPageComponent from '@/components/landing';

export default {
  components: { LandingPageComponent },
  async asyncData({ app }) {
    const cur_pageUrl = window.location.pathname.replace(/\//g, '');
    const response = await app.$axios.$post(
      'https://runflow.built.io/run/27mqbwl0xi?sync=true',
      { pageUrl: cur_pageUrl }
    );
    return { pagetypeInfo: response.pagetypeInfo };
  },
  data() {
    return {
      onFormSubmitUrl: 'https://runflow.built.io/run/1DGm8LpMUw?sync=true',
      url: 'eod4',
    };
  },
};
</script>
