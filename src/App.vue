<template>
  <div>
    <!-- <span>{{ md }}</span> -->
    <header>
      <CommonHeader></CommonHeader>
    </header>
    <main>
      <CommonMain
        title="X.509 인증서 발급"
        content="인증서 발급을 체험해볼 수 있습니다."
      >
        <template #content>
          <div class="d-flex justify-content-between mt-2">
            <span>키 길이: </span>
            <select v-model="keyLength">
              <option value="1024">1024</option>
              <option value="2048">2048</option>
              <option value="3072">3072</option>
              <option value="4096">4096</option>
            </select>
          </div>
          <div>
            <div class="d-flex justify-content-between mt-2">
              <div>이름</div>
              <input v-model="attrs[0].value" placeholder="Common Name">
            </div>
            <div class="d-flex justify-content-between mt-2">
              <div>국가</div>
              <input v-model="attrs[1].value" placeholder="Country">
            </div>
            <div class="d-flex justify-content-between mt-2">
              <div>광역시도</div>
              <input v-model="attrs[2].value" placeholder="State">
            </div>
            <div class="d-flex justify-content-between mt-2">
              <div>시군</div>
              <input v-model="attrs[3].value" placeholder="City">
            </div>
            <div class="d-flex justify-content-between mt-2">
              <div>기관명</div>
              <input v-model="attrs[4].value" placeholder="Organization">
            </div>
            <div class="d-flex justify-content-between mt-2">
              <div>부서명</div>
              <input v-model="attrs[5].value" placeholder="Organizational Unit">
            </div>
          </div>
          <div>
            <button @click="generateKeyPair" type="button" class="mt-4 btn btn-outline-secondary btn-lg px-4">
              키 쌍 생성
            </button>
          </div>
          <div class="mt-5 d-flex flex-column">
            <div class="text-start lead">인증기관 공개키</div>
            <textarea v-model="this.publicKey" readonly class="text-start mt-2 h-250"></textarea>
          </div>
          <div class="mt-5 d-flex flex-column">
            <div class="text-start lead">인증기관 개인키</div>
            <textarea v-model="this.privateKey" readonly class="text-start mt-2 h-250"></textarea>
          </div>
          <div class="mt-5 d-flex flex-column">
            <div class="text-start lead">발급된 인증기관 인증서</div>
            <textarea v-model="this.certificate" readonly class="text-start mt-2 h-250"></textarea>
          </div>
        </template>
      </CommonMain>
      <CommonMainDivider/>

      <CommonMain
        title="전자서명 로그인"
        content="전자서명 로그인을 체험해 볼 수 있습니다."
      />
      <CommonMainDivider/>

      <CommonMain
        title="전자봉투 송신"
        content="전자봉투 송신을 체험해 볼 수 있습니다."
      />
      <CommonMainDivider/>

      <CommonMain
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
    return {
      keyLength: '2048', // 키 길이
      certificate: '',
      publicKey: '',
      privateKey: '',
      attrs: [
        { name: 'commonName', value: '여승철' },
        { name: 'countryName', value: 'KR' },
        { shortName: 'ST', value: '경기도' },
        { name: 'localityName', value: '고양시' },
        { name: 'organizationName', value: '중부대학교' },
        { shortName: 'OU', value: '정보보호학과' }
      ],
    };
  },

  mounted() {
    // md = forge.md.md5.create()
    // md.update('The quick brown fox jumps over the lazy dog')
  },

  methods: {
    generateKeyPair() {
      const keyLength = parseInt(this.keyLength); // 키 길이
      const now = new Date();

      // 새로운 키 쌍 생성
      const keys = forge.pki.rsa.generateKeyPair(keyLength);

      // 인증서 객체 생성
      const cert = forge.pki.createCertificate();

      // 인증서 기본 정보 설정
      cert.publicKey = keys.publicKey; // 공개키
      cert.serialNumber = '01';
      // 현재 날짜 기준으로 유효 기간 설정(1년)
      cert.validity.startDate = now
      cert.validity.endDate = new Date(now.getFullYear() + 1, now.getMonth(), now.getDate());

      // 인증서 주체 설정
      const attrs = [
        { name: 'commonName', value: this.attrs[0].value },
        { name: 'countryName', value: this.attrs[1].value },
        { shortName: 'ST', value: this.attrs[2].value },
        { name: 'localityName', value: this.attrs[3].value },
        { name: 'organizationName', value: this.attrs[4].value },
        { shortName: 'OU', value: this.attrs[5].value }
      ]
      cert.setSubject(attrs);

      // 인증서 발급자 설정(자기 자신이 서명)
      cert.setIssuer(attrs);

      // 인증서 서명
      cert.sign(keys.privateKey, forge.md.sha256.create());

      const pemCertificate = forge.pki.certificateToPem(cert);
      const pemPrivateKey = forge.pki.privateKeyToPem(keys.privateKey);
      
      // 인증서, 사설키, 공개키
      this.certificate = pemCertificate;
      this.privateKey = pemPrivateKey;
      this.publicKey = forge.pki.publicKeyToPem(keys.publicKey);  // PEM형식으로 변환해서 보여줌
    }
  },
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

.h-250 {
  height: 250px;
}
</style>
