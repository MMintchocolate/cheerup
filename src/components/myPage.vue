<template>
  <div class="myPageMain">
    <div class="myPageCard">
      <div class="logo">
        <img src="../assets/logo.png" />
        <h2 class="myPageTitle" style="color: #000080">내 정보</h2>
      </div>
      <hr />
      <div class="myPageCardContents">
        <table class="myPageTable">
          <tr>
            <th>닉네임</th>
            <td>{{ userData ? userData.nickname : "로딩 중..." }}</td>
          </tr>
          <tr>
            <th>아이디</th>
            <td>{{ userData ? userData.id : "로딩 중..." }}</td>
          </tr>
        </table>
      </div>
      <hr />
      <h3>내가 쓴 글</h3>
      <table class="myPageMyReivewTable">
        <tr v-for="(post, index) in userData.posts" :key="index">
          <th>{{ post.title }}</th>
          <td>{{ post.date }}</td>
        </tr>
        <tr v-if="!userData.posts || userData.posts.length === 0">
          <td colspan="2">게시물이 없습니다.</td>
        </tr>
      </table>
    </div>
  </div>
</template>
<script>
import axios from "axios";

export default {
  name: "myPage",
  props: ["userId"],

  data() {
    return {
      userData: {},
    };
  },
  created() {
    this.fetchUserData(this.userId);
    console.log("mypage:", this.userId);
  },
  methods: {
    async fetchUserData(userId) {
      try {
        const response = await axios.get(`/api/mypage/${userId}`);
        this.userData = response.data;

        console.log("mypage:", this.userData);
      } catch (error) {
        console.error(error);
      }
    },
  },
};
</script>

<style>
.myPageMain {
  text-align: center;
}
/* 카드형식으로 만들기 */
.myPageCard {
  background-color: white;
  max-width: 400px;
  margin: 20px auto;
  padding: 20px;
  border: 1px solid white;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  /* display: flex;
  flex-direction: column;
  text-align: center;
  justify-items: center;
  align-items: center; */
}

/*내정보 아래 닉네임 아이디 취업상태 */
.myPageTable {
  padding: 10px;
  width: 100%;
  text-align: left;
}
.myPageTable > hr {
  margin: 15px;
}

/*내가쓴글 */
.myPageMyReivewTable {
  padding: 10px;
  width: 100%;
  text-align: left;
  font-weight: normal;
}
</style>
