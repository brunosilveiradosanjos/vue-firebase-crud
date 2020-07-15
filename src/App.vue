<template>
  <div id="app">
    <div class="container">
      <div class="row">
        <div class="col-xs-12 col-sm-8 col-sm-offset-2 col-md-6 col-md-offset-3">
          <h1>Http</h1>
          <div class="form-group">
            <label>Username</label>
            <input type="text" class="form-control" v-model="user.username" />
          </div>
          <div class="form-group">
            <label>Mail</label>
            <input type="text" class="form-control" v-model="user.email" />
          </div>
          <button class="btn btn-primary" @click="submit">Submit</button>
          <hr />
          <button class="btn btn-primary" @click="fetchData">Get Data</button>
          <br />
          <br />
          <ul class="list-group">
            <li
              class="list-group-item"
              v-for="(user,index) in users"
              :key="index"
            >{{index}} {{user.username}} - {{user.email}}</li>
          </ul>
        </div>
      </div>
    </div>
    <img alt="Vue logo" src="./assets/logo.png" />
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      user: {
        username: "",
        email: ""
      },
      users: []
    };
  },
  methods: {
    // async / await
    async submit() {
      try {
        const res = await this.$http.post(
          "https://vuejs-http-1f05a.firebaseio.com/data.json",
          this.user
        );
        console.log(res);
      } catch (error) {
        console.log(error);
      }

      // Promise
      // this.$http
      //   .post("https://vuejs-http-1f05a.firebaseio.com/data.json", this.user)
      //   .then(
      //     res => {
      //       console.log(res);
      //     },
      //     error => {
      //       console.error(error);
      //     }
      //   );
      // console.log(this.user);
    },
    fetchData() {
      // try {
      //   const res = await this.$http.get(
      //     "https://vuejs-http-1f05a.firebaseio.com/data.json"
      //   );
      //   console.log(res.json());
      // } catch (error) {
      //   console.log(error);
      // }

      this.$http
        .get("https://vuejs-http-1f05a.firebaseio.com/data.json")
        .then(res => {
          return res.json();
        })
        .then(data => {
          var resultArray = [];
          for (let key in data) {
            resultArray.push(data[key]);
          }
          this.users = resultArray;
        });
    }
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
