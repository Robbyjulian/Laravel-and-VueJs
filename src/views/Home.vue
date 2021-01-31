<template>
  <div class="home">
  
  <HeaderRj />
  <HeroRj />
  <WomanRj />
  <InstaRj />
  <PartnerRj />
  <FooterRj />

  </div>
</template>

<script>
// @ is an alias to /src
// import HelloWorld from '@/components/HelloWorld.vue'
import HeaderRj from '@/components/HeaderRj.vue';
import HeroRj from '@/components/HeroRj.vue';
import WomanRj from '@/components/WomanRj.vue';
import InstaRj from '@/components/InstaRj.vue';
import PartnerRj from '@/components/PartnerRj.vue';
import FooterRj from '@/components/FooterRj.vue';


export default {
  name: 'Home',
  components: {
    HeaderRj,
    HeroRj,
    WomanRj,
    InstaRj,
    PartnerRj,
    FooterRj
  }
}
</script>
