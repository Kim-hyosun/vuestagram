<template>
  <div class="header">
    <ul class="header-button-left">
      <li>Cancel</li>
    </ul>
    <ul class="header-button-right">
      <li v-if="step == 1" @click="step++">Next</li>
      <li v-if="step == 2" @click="publish">발행</li>
    </ul>
    <img src="./assets/logo.png" class="logo" />
  </div>

  <Container
    :postData="postData"
    :step="step"
    :upImage="upImage"
    @write="작성한글 = $event"
  />
  <button @click="more">더보기</button>

  <!-- <div style="margin-top: 100px">
    <button @click="step = 0">버튼0</button>
    <button @click="step = 1">버튼1</button>
    <button @click="step = 2">버튼2</button>
    <div v-if="step == 0">내용0</div>
    <div v-if="step == 1">내용1</div>
    <div v-if="step == 2">내용2</div>
  </div> -->

  <div class="footer">
    <ul class="footer-button-plus">
      <input
        @change="upload()"
        multiple
        type="file"
        id="file"
        class="inputfile"
      />
      <label for="file" class="input-plus">+</label>
    </ul>
  </div>
</template>

<script>
import Container from "./components/Container.vue";
import postData from "./assets/data";
import axios from "axios";
export default {
  name: "App",
  components: {
    Container,
  },
  data() {
    return {
      step: 0,
      postData,
      moreNum: 0,
      upImage: "",
      작성한글: "",
    };
  },
  methods: {
    publish() {
      let 내게시물 = {
        name: "Kim Hyun",
        userImage: "https://picsum.photos/100?random=1",
        postImage: this.upImage,
        likes: 36,
        date: "May 15",
        liked: false,
        content: this.작성한글,
        filter: "perpetua",
      };
      this.postData.unshift(내게시물);
      this.step = 0;
    },
    more() {
      axios
        .get(`https://codingapple1.github.io/vue/more${this.moreNum}.json`)
        .then((res) => {
          //요청성공시 실행할 코드
          console.log(res.data);
          this.postData.push(res.data);
          this.moreNum++;
        });
    },
    upload(e) {
      let file = e.target.files;
      let url = URL.createObjectURL(file[0]);
      console.log(url);
      this.upImage = url;
      this.step++;
    },
  },
};
</script>

<style>
body {
  margin: 0;
}
ul {
  padding: 5px;
  list-style-type: none;
}
.logo {
  width: 22px;
  margin: auto;
  display: block;
  position: absolute;
  left: 0;
  right: 0;
  top: 13px;
}
.header {
  width: 100%;
  height: 40px;
  background-color: white;
  padding-bottom: 8px;
  position: sticky;
  top: 0;
}
.header-button-left {
  color: skyblue;
  float: left;
  width: 50px;
  padding-left: 20px;
  cursor: pointer;
  margin-top: 10px;
}
.header-button-right {
  color: skyblue;
  float: right;
  width: 50px;
  cursor: pointer;
  margin-top: 10px;
}
.footer {
  width: 100%;
  position: sticky;
  bottom: 0;
  padding-bottom: 10px;
  background-color: white;
}
.footer-button-plus {
  width: 80px;
  margin: auto;
  text-align: center;
  cursor: pointer;
  font-size: 24px;
  padding-top: 12px;
}
.sample-box {
  width: 100%;
  height: 600px;
  background-color: bisque;
}
.inputfile {
  display: none;
}
.input-plus {
  cursor: pointer;
}
#app {
  box-sizing: border-box;
  font-family: "consolas";
  margin-top: 60px;
  width: 100%;
  max-width: 460px;
  margin: auto;
  position: relative;
  border-right: 1px solid #eee;
  border-left: 1px solid #eee;
}
</style>
