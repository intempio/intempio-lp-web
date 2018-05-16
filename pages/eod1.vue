<template>
      <LandingPageComponent :title="title"/>
</template>

<script>
import LandingPageComponent from '@/components/landing';
export default {
  components: { LandingPageComponent },
  data() {
    return {
      title: 'SPINRAZA® (nusinersen) <br> Expert on Demand',
    };
  },
};
</script>

<style scoped>
</style>
