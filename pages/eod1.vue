<template>
      <LandingPageComponent :client="client"/>
</template>

<script>
import CLIENTS from '@/utils/clients';
import LandingPageComponent from '@/components/landing';

export default {
  components: { LandingPageComponent },
  async asyncData({ app }) {
    const response = await app.$axios.$post(
      'https://runflow.built.io/run/27mqbwl0xi?sync=true',
      { pageUrl: 'eod1' }
    );
    return { response };
  },
  created() {
    const { brand, page, title, acLink } = this.response.pagetypeInfo;
    window.history.replaceState({}, document.title, `/${brand}/${page}`);
    this.client = CLIENTS[brand];
  },
  data() {
    return {
      client: {
        logo: '',
        title: '',
        footerRighText: '',
        privacyPolicy: '',
        termsOfUse: '',
        importantSafetyInformation: '',
        prescribingInformation: '',
      },
    };
  },
};
</script>
