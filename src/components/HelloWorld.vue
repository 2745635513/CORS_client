<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <button @click="hello">axios请求</button>
    <button @click="imgRequest">图片请求</button>
    <button @click="jsonpRequest">jsonp请求</button>
    <button @click="corsRequest">cors请求</button>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "HelloWorld",
  props: {
    msg: String,
  },
  methods: {
    hello() {
      axios.get("http://127.0.0.1:3000/", {
        params: {
          ID: 12345,
        },
      });
    },
    imgRequest() {
      const img = new Image();
      img.src = "http://localhost:3000?id=www";
    },
    jsonpRequest() {
      const script = document.createElement("script");
      const funName = `jsonp${Math.random()
        .toString(16)
        .substr(2)}`;
      console.log(funName);
      script.src = `http://127.0.0.1:3000/jsonp?funName=${funName}`;
      window[funName] = (data) => {
        console.log(data);
        document.body.removeChild(script);
        window[funName] = null;
      };
      document.body.appendChild(script);
    },
    corsRequest() {
      axios({
        url: "http://localhost:3000/cors",
        method: "post",
        withCredentials: true,
      }).then((res) => {
        console.log(res.data);
      });
    },
  },
};
</script>
