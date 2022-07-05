<template>
  <div class="about">
      <div id="naver_id_login"></div>
  </div>
</template>

<script>
export default{
  name:'Login',
  mounted() {
    const naver_id_login = new window.naver_id_login("gougRQEKuJnWMxeoKYvv", "http://localhost:8080/callback");
    var state = naver_id_login.getUniqState();
    naver_id_login.setButton("white", 2,40);
    naver_id_login.setDomain("http://localhost:8080");
    naver_id_login.setState(state);
    //팝업으로 로그인 보여줄때 설정
    // naver_id_login.setPopup();
    naver_id_login.init_naver_id_login();
  }
}

</script>