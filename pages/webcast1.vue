<template>
      <LandingPageComponent :client="client"/>
</template>

<script>
import CLIENTS from '@/utils/clients';
import LandingPageComponent from '@/components/landing';

export default {
  components: { LandingPageComponent },
  data() {
    return {
      client: CLIENTS.Biogen,
    };
  },
};
</script>

<style scoped>
</style>
