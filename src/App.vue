<template>
  <div>
    <!-- <span>{{ md }}</span> -->
    <header>
      <CommonHeader></CommonHeader>
    </header>
    <!-- slot사용 예정 -->
    <main>
      <CommonMain
        id="certificate"
        title="인증서 발급"
        content="인증서 발급을 체험해볼 수 있습니다."
      />
      <CommonMainDivider/>

      <CommonMain
        id="transmit"
        title="전자봉투 송신"
        content="전자봉투 송신을 체험해 볼 수 있습니다."
      />
      <CommonMainDivider/>

      <CommonMain
        id="receive"
        title="전자봉투 수신"
        content="전자봉투 수신을 체험해 볼 수 있습니다."
      />
      <CommonMainDivider/>
    </main>

    <footer class="container">
      <CommonFooter></CommonFooter>
    </footer>
  </div>
</template>

<script>
import CommonHeader from "./components/CommonHeader.vue";
import CommonMain from './components/CommonMain.vue';
import CommonMainDivider from './components/CommonMainDivider.vue';
import CommonFooter from "./components/CommonFooter.vue";

export default {
  name: "App",

  components: {
    CommonHeader,
    CommonMain,
    CommonMainDivider,
    CommonFooter,
  },

  data() {
    return {};
  },

  mounted() {
    // md = forge.md.md5.create()
    // md.update('The quick brown fox jumps over the lazy dog')
  },

  methods: {},
};
</script>
<style scoped>
@import url(./assets/css/normalize.css);
.bd-placeholder-img {
  font-size: 1.125rem;
  text-anchor: middle;
  -webkit-user-select: none;
  -moz-user-select: none;
  user-select: none;
}

@media (min-width: 768px) {
  .bd-placeholder-img-lg {
    font-size: 3.5rem;
  }
}
</style>
