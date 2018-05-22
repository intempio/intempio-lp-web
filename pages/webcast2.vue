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
    const response = await app.$axios.$post(
      'https://runflow.built.io/run/2DK1vAkadY?sync=true',
      { pageUrl: 'webcast2' }
    );
    return { pagetypeInfo: response.pagetypeInfo };
  },
  data() {
    return {
      onFormSubmitUrl: 'https://runflow.built.io/run/2RfHUEtCcg?sync=true',
      url: 'webcast2',
    };
  },
  head() {
    return {
      title: 'Webcast | Intempio',
    };
  },
};
</script>
