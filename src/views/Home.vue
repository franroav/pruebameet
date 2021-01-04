<template>
  <div class="home">
    <Header />
    <Articles />
    <Contact />
  </div>
</template>

<script>
import Header from "@/components/Header.vue";
import Articles from "@/components/Articles.vue";
import Contact from "@/components/Contact.vue";

export default {
  name: "Home",
  components: {
    Header,
    Articles,
    Contact,
  },
};
</script>