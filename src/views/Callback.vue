<template>
  <div>
    <div id="naver_id_login"></div>
    <button @click="getProfile()">프로필 보기</button>
    <p>email: {{this.email}}</p>
    <p>name: {{this.name}}</p>
    <p>gender: {{this.gender}}</p>
    <p>birthday: {{this.birthday}}</p>
  </div>
</template>

<script>

import axios from 'axios'

export default{
  name:'Login',
  data(){
    return{
      naver_id_login: '',
      userNaverToken: '',
      email: '',
      name: '',
      gender: '',
      birthday: ''
    }
  },
  created() {
    this.naver_id_login = new window.naver_id_login("gougRQEKuJnWMxeoKYvv", "http://localhost:8080/callback");
    // 접근 토큰 값 출력
    console.log("access token", this.naver_id_login.getAccessToken()); // 정상적 로그인이 된 경우 access token값 출력
    // 네이버 사용자 프로필 조회 이후 프로필 정보를 처리할 callback function

    this.naver_id_login.get_naver_userprofile(this.getProfile());
  },
  mounted() {
    console.log(this.naver_id_login.getProfileData('email'))
  },
  methods: {
    getProfile(){
      this.email = this.naver_id_login.getProfileData('email');
      this.name = this.naver_id_login.getProfileData('name');
      this.gender = this.naver_id_login.getProfileData('gender');
      this.birthday = this.naver_id_login.getProfileData('birthday');
      this.mobile = this.naver_id_login.getProfileData('mobile');

    }
  }
}

</script>