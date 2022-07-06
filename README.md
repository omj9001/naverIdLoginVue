# naverIdLoginVue
네이버 아이디 로그인(네아로) 오픈 API를 vue를 통해 사용해봤습니다.

해당 프로젝트는 Client 시점의 네아로 적용 테스트입니다.

## Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

## Page 구성
> * Home - 메인 화면
> * Login - Naver Login 버튼을 보여주는 화면(이 버튼을 보여줘야 해당 페이지에서 네아로 플러그인을 정상적으로 사용할 수 있다.)
> * Callback - Naver Login 요청에 따른 응답을 받는 화면이다. (해당 화면에서 Access token을 받을수 있고 이 token으로 사용자의 제공 정보를 받아올 수 있다.)

## 네아로 적용 process
네이버 아이디로 로그인을 적용하기 위해서는 아래의 단계를 따라야 합니다.

> * import naverLogin_implicit-1.0.3.js
> * naver_id_login 태그 추가
> * callback page 구현

### import naverLogin_implicit-1.0.3.js
vue cli를 통해 2.X번대의 프로젝트를 생성합니다.(기존에 프로젝트에 사용하셔도 무방합니다)

생성한 프로젝트의 public/index.html 파일에 아래 스크립트를 추가합니다.

```
<script type="text/javascript" src="https://static.nid.naver.com/js/naverLogin_implicit-1.0.3.js" charset="utf-8"></script>
<script type="text/javascript" src="http://code.jquery.com/jquery-1.11.3.min.js"></script>
```

### naver_id_login 태그 추가
네이버 아이디로 로그인 버튼을 추가할 페이지에 naver_id_login 태그를 추가합니다.(해당 프로젝트에서는 Login.vue 페이지에서 버튼 노출)

```
<template>
  <div class="about">
      <div id="naver_id_login"></div>
  </div>
</template>

<script>
export default{
  name:'Login',
  mounted() {
    const naver_id_login = new window.naver_id_login("CLIENT_ID", "http://localhost:8080/callback");
    var state = naver_id_login.getUniqState();
    naver_id_login.setButton("white", 2,40);
    naver_id_login.setDomain("http://localhost:8080");
    naver_id_login.setState(state);
    //팝업으로 로그인 보여줄때 설정
    naver_id_login.init_naver_id_login();
  }
}

</script>
```

### callback page 구현
네이버 아이디로 로그인 버튼을 눌러 로그인이 되면 callback 페이지로 이동하게 된다.

```
const naver_id_login = new window.naver_id_login("CLIENT_ID", "http://localhost:8080/callback");
```

http://localhost:8080/callback으로 이동하게 되는데 이때 acesstoken이 부여된다.

아래는 callback.vue 소스 구현 내용

```
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
    this.naver_id_login = new window.naver_id_login("CLIENT_ID", "http://localhost:8080/callback");
    // 접근 토큰 값 출력
    console.log("access token", this.naver_id_login.getAccessToken()); // 정상적 로그인이 된 경우 access token값 출력
    // 네이버 사용자 프로필 조회 이후 프로필 정보를 처리할 callback function
    this.naver_id_login.get_naver_userprofile(this.getProfile());
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
```

## 네아로 적용 했을때 한계점
get_naver_userprofile(call function) 함수가 API 서버와의 통신 결과 값을 받고 call function을 해줘야 하는데 그렇지 못해서 부득이 하게 버튼을 두었다.

