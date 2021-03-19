<template>
  <div id="app">
    <h2>1⃣️.获取文件md5值</h2>
    <p>说明：选择文件上传即可在页面上现实文件md值</p>
    <input type="file" @change="onOne" />
    <div>md5值：{{ md5 }}</div>

    <h2>2⃣️.csv文件转二维数组</h2>
    <p>说明：选择csv文件上传即可将其转为二维数组（见控制台）</p>
    <a href="http://qiniu.kareza.cn/normal.csv">测试数据</a>
    <input type="file" accept=".csv" @change="onTwo" />
  </div>
</template>

<script>
import SparkMD5 from "spark-md5";
import Papa from "papaparse";

export default {
  name: "app",
  data() {
    return {
      md5: "****",
    };
  },
  methods: {
    onOne: function (event) {
      var _this = this;
      var fileReader = new FileReader();
      var spark = new SparkMD5.ArrayBuffer();
      // 获取文件二进制数据
      fileReader.readAsArrayBuffer(event.target.files[0]);
      fileReader.onload = function (e) {
        spark.append(e.target.result);
        var md5 = spark.end();
        _this.md5 = md5;
      };
    },
    onTwo: function (event) {
      Papa.parse(event.target.files[0], {
        complete: function (results) {
          console.log(results);
        },
      });
    },
  },
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

h1,
h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
</style>
