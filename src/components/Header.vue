<template>
  <v-app>
    <section class="app-header">
      <!--HEADER BACKGROUND-->
      <div class="header-img animated fadeIn"></div>
      <div class="container-fluid">
        <div class="row my-5">
          <div class="offset-md-1 col-lg-6 col-md-6 col-sm-6 col-xs-6">
            <em class="header-logo animated bounceInLeft">Logo</em>
          </div>
          <div
            class="col-lg-5 col-md-5 col-sm-5 col-xs-5 social animated bounceInDown"
          >
            <v-tooltip top>
              <template v-slot:activator="{ on }">
                <span
                  v-on="on"
                  class="badge float-right btn-circle sdw pointer mr-3"
                  @click="redirectUrl('www.youtube.com')"
                >
                  <v-btn elevation="2" fab outlined color="#D8AD3D" href=""
                    ><i class="fa fa-youtube-play"></i>
                  </v-btn>
                </span>
              </template>
              <span>Youtube</span>
            </v-tooltip>
            <v-tooltip top>
              <template v-slot:activator="{ on }">
                <span
                  v-on="on"
                  class="badge float-right sdw pointer mr-3"
                  @click="redirectUrl('www.instagram.com')"
                >
                  <v-btn elevation="2" fab outlined color="#B72C2C" href=""
                    ><i class="fa fa-instagram"></i>
                  </v-btn>
                </span>
              </template>
              <span>Instagram</span>
            </v-tooltip>
            <v-tooltip top>
              <template v-slot:activator="{ on }">
                <span
                  v-on="on"
                  class="badge float-right sdw pointer mr-3"
                  @click="redirectUrl('www.facebook.com')"
                >
                  <v-btn elevation="2" fab outlined color="#009CD9">
                    <i class="fa fa-facebook"></i>
                  </v-btn>
                </span>
              </template>
              <span>Facebook</span>
            </v-tooltip>
          </div>
        </div>
      </div>
    </section>
    <section class="header-main">
      <div class="row my-5">
        <h2 class="header-main-text animated bounceInLeft">
          El secreto <br />
          de tu cocina
        </h2>
      </div>
      <div class="row">
        <div class="header-brush animated bounceInLeft"></div>
      </div>
      <!--HERO-->
      <div class="row">
        <div class="header-gradient"></div>
      </div>
    </section>
  </v-app>
</template>

<script>
import axios from "axios";

export default {
  name: "Header",
  data: () => ({
    data: {},
  }),
  mounted() {},
  created() {
    this.GetArticleList();
  },
  methods: {
    redirectUrl(url) {
      //window.location.href = url;
      this.$router.push(url);
    },
    GetArticleList() {
      try {
        var vm = this;
        let datos = axios
          .get(`https://5eed24da4cbc340016330f0d.mockapi.io/api/articles`)
          .then((articles) => {
            vm.articleList(articles.data);
          });

        //;

        //this.config.headers = datos.data.config.headers
        //let datoss = await axios.post(`http://localhost/apiMuti/public/api/register`, nuevoUser, this.config.headers);
        console.log(datos);
      } catch (error) {
        console.log(error);
      }
    },
    articleList(articles) {
      console.log(articles);
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
/* b2 1 */
.header-img {
  position: absolute;
  width: 1536px;
  height: 805px;
  left: -96px;
  top: -14px;
  background: url("../assets/header_background.png");
  background-repeat: no-repeat;
}

.header-logo {
  /* Logo */

  color: #3f454a;
  font-family: "Caveat";
  //font-family: "Comic Sans MS", cursive, sans-serif;
  font-size: 90px;
  font-weight: bold;
  letter-spacing: -1px;
  line-height: 1;
  text-align: center;

  animation-delay: 0.5s;
  animation-duration: 1.5s;

  /* #24272A */
}

.header-main-text {
  /* El secreto de tu cocina */

  position: absolute;
  font-family: "Caveat";
  font-style: normal;
  font-weight: bold;
  font-size: 120px;
  line-height: 100px;
  margin-top: 150px;
  padding-left: 90px;
  display: flex;
  align-items: center;
  color: #3f454a;
  animation-delay: 0.5s;
  animation-duration: 1.5s;
  z-index: 1;
}

.header-brush {
  position: absolute;
  width: 1312px;
  height: 174px;
  left: -80px;
  top: 428.62px;
  background: url("../assets/brush_header.png");
  background-repeat: no-repeat;
  transform: rotate(2.4deg);
  animation-delay: 0.5s;
  animation-duration: 1.5s;
}

/* Rectangle 180 */

.header-gradient {
  position: absolute;
  width: 1447px;
  height: 253px;
  left: 0px;
  top: 560px;

  background: linear-gradient(
    0deg,
    #ffffff 0%,
    #ffffff 32.22%,
    rgba(255, 255, 255, 0.71) 61.99%,
    rgba(255, 255, 255, 0.0729167) 93.72%,
    rgba(255, 255, 255, 0) 93.72%
  );
  z-index: 1;
}

.social {
  animation-delay: 0.5s;
  animation-duration: 1.5s;
}
</style>
