<template>
  <v-app>
    <div class="container" style="margin-top: 240px">
      <div class="row article-row-title animated bounceInUp">
        <div class="col-md-8 offset-md-2 text-center">
          <h2 class="article-title">Nuestros {{ title }}</h2>
          <img class="my-5" src="../assets/brush_background.png" />
        </div>
      </div>
      <div class="row">
        <!--MENU-->
        <div class="col-md-3 article-col-menu animated bounceInLeft">
          <v-card class="mx-auto" max-width="300" tile>
            <v-list dense>
              <v-list-item-group color="warning">
                <v-list-item>
                  <v-list-item-content>
                    <v-list-item-title @click="getArticleList()"
                      >TODOS
                      <i
                        class="fa fa-arrow-right float-right"
                        style="color: #d8ad3d"
                      ></i>
                    </v-list-item-title>
                  </v-list-item-content>
                </v-list-item>
                <v-list-item>
                  <v-list-item-content>
                    <v-list-item-title @click="getProductList('Productos')"
                      >PRODUCTOS</v-list-item-title
                    >
                  </v-list-item-content>
                </v-list-item>
                <v-list-item>
                  <v-list-item-content>
                    <v-list-item-title @click="getRecipeList('Recetas')"
                      >RECETAS</v-list-item-title
                    >
                  </v-list-item-content>
                </v-list-item>
                <v-list-item>
                  <v-list-item-content>
                    <v-list-item-title @click="getAdviseList('Consejos')"
                      >CONSEJOS</v-list-item-title
                    >
                  </v-list-item-content>
                </v-list-item>
              </v-list-item-group>
            </v-list>
          </v-card>
        </div>
        <div class="col-md-9 article-col-cards animated bounceInRight">
          <div :class="visibility">
            <!--CARD ONE-->
            <div class="col-md-4">
              <div class="card text-center animate-cards animated fadeIn">
                <div class="card-header">
                  <img
                    :src="arrArticle[0].image"
                    width="220px"
                    height="204px"
                  />
                </div>
                <div class="card-body">
                  <strong class="article-card-title">{{
                    arrArticle[0].title
                  }}</strong>
                  <p class="article-card-body pt-2">
                    {{ arrArticle[0].content }}
                  </p>
                </div>
              </div>
            </div>
            <!--CARD TWO-->
            <div class="col-md-4">
              <div class="card text-center animate-cards animated fadeIn">
                <div class="card-header">
                  <img
                    :src="arrArticle[1].image"
                    width="220px"
                    height="204px"
                  />
                </div>
                <div class="card-body">
                  <strong class="article-card-title">{{
                    arrArticle[1].title
                  }}</strong>
                  <p class="article-card-body pt-2">
                    {{ arrArticle[1].content }}
                  </p>
                </div>
              </div>
            </div>
            <!--CARD THREE-->
            <div class="col-md-4">
              <div class="card text-center animate-cards animated fadeIn">
                <div class="card-header">
                  <img
                    :src="arrArticle[2].image"
                    width="220px"
                    height="204px"
                  />
                </div>
                <div class="card-body">
                  <strong class="article-card-title">{{
                    arrArticle[2].title
                  }}</strong>
                  <p class="article-card-body pt-2">
                    {{ arrArticle[2].content }}
                  </p>
                </div>
              </div>
            </div>
          </div>
          <div :class="visibility" v-if="arrArticle.length > 0">
            <!--CARD FOUR-->
            <div class="col-md-4">
              <div class="card text-center animate-cards animated fadeIn">
                <div class="card-header">
                  <img
                    :src="arrArticle[3].image"
                    width="220px"
                    height="204px"
                  />
                </div>
                <div class="card-body">
                  <strong class="article-card-title">{{
                    arrArticle[3].title
                  }}</strong>
                  <p class="article-card-body pt-2">
                    {{ arrArticle[3].content }}
                  </p>
                </div>
              </div>
            </div>
            <!--CARD FIVE-->
            <div class="col-md-4">
              <div class="card text-center animate-cards animated fadeIn">
                <div class="card-header">
                  <img
                    :src="arrArticle[4].image"
                    width="220px"
                    height="204px"
                  />
                </div>
                <div class="card-body">
                  <strong class="article-card-title">{{
                    arrArticle[4].title
                  }}</strong>
                  <p class="article-card-body pt-2">
                    {{ arrArticle[4].content }}
                  </p>
                </div>
              </div>
            </div>
            <div class="col-md-4"></div>
          </div>
        </div>
      </div>
    </div>
  </v-app>
</template>

<script>
import axios from "axios";

export default {
  name: "Articles",
  data: () => ({
    title: "Articulos",
    arrArticle: [],
    arrProductos: [],
    arrRecetas: [],
    arrConsejos: [],
    visibility: "row hide",
  }),
  mounted() {
    this.title = "articulos";
    try {
      var vm = this;
      let datos = axios
        .get(`https://5eed24da4cbc340016330f0d.mockapi.io/api/articles`)
        .then((articles) => {
          vm.articleList(articles.data);
        });
      //console.log(datos);
    } catch (error) {
      console.log(error);
    }
  },
  created() {},
  methods: {
    redirectUrl(url) {
      //window.location.href = url;
      this.$router.push(url);
    },
    articleList(articles) {
      this.articleStorage(articles);
    },
    getProductList(Productos) {
      try {
        var vm = this;
        this.title = "productos";
        let datos = axios
          .get(
            `https://5eed24da4cbc340016330f0d.mockapi.io/api/articles?filter=${Productos}`
          )
          .then((productos) => {
            vm.productList(productos.data);
          });
      } catch (error) {
        console.log(error);
      }
    },
    productList(producto) {
      this.articleStorage(producto);
    },
    getRecipeList() {
      try {
        var vm = this;
        this.title = "recetas";
        let datos = axios
          .get(
            `https://5eed24da4cbc340016330f0d.mockapi.io/api/articles?filter=Recetas`
          )
          .then((recetas) => {
            vm.recipeList(recetas.data);
          });
      } catch (error) {
        console.log(error);
      }
    },
    recipeList(recipe) {
      this.articleStorage(recipe);
    },
    getAdviseList(advise) {
      try {
        var vm = this;
        this.title = "consejos";
        let datos = axios
          .get(
            `https://5eed24da4cbc340016330f0d.mockapi.io/api/articles?filter=${advise}`
          )
          .then((consejo) => {
            vm.adviseList(consejo.data);
          });
      } catch (error) {
        console.log(error);
      }
    },
    adviseList(consejos) {
      this.articleStorage(consejos);
    },
    articleStorage(data) {
      let arrData = [];

      for (let i = 0; i < data.length; i++) {
        if (i < 6) {
          arrData.push(data[i]);
        }
      }

      if (localStorage.getItem("article") !== null && data.lenght !== 0) {
        this.arrArticle = [];
        localStorage.removeItem("article");

        localStorage.setItem("article", JSON.stringify(arrData));

        let articleStorage = JSON.parse(localStorage.getItem("article"));

        this.arrArticle = articleStorage;
        this.visibility = "row mt-2";
      } else if (
        localStorage.getItem("article") === null &&
        data.lenght !== 0
      ) {
        localStorage.setItem("article", JSON.stringify(arrData));

        this.arrArticle = arrData;
        this.visibility = "row";
      }
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
/* b2 1 */
.header {
  position: absolute;
  width: 1536px;
  height: 805px;
  left: -96px;
  top: -14px;

  //background: url(b2.png);
}

.article-title {
  /* Nuestros artículos */

  position: absolute;
  left: 28.68%;
  right: 28.68%;
  top: 1.82%;
  bottom: 65.52%;

  /* H2 */

  font-family: Caveat;
  font-style: normal;
  font-weight: bold;
  font-size: 80px;
  line-height: 38px;
  /* or 47% */

  display: flex;
  align-items: center;
  text-align: center;

  /* #24272A */

  color: #3f454a;
}

.article-menu {
  /* todos PRODUCTOS RECETAS CONSEJOS */

  position: absolute;
  width: 198px;
  height: 177px;
  left: 154px;
  top: 990px;

  font-family: Open Sans;
  font-style: normal;
  font-weight: bold;
  font-size: 12px;
  line-height: 23px;
  /* or 192% */

  letter-spacing: 0.03em;
  text-transform: uppercase;

  /* #24272A */

  color: #3f454a;
}

.article-card-title {
  font-family: Open Sans;
  font-style: normal;
  font-weight: bold;
  font-size: 18px;
  line-height: 24px;
  /* or 133% */

  letter-spacing: 0.01em;

  /* #24272A */

  color: #3f454a;
}

.article-card-body {
  font-family: Open Sans;
  font-style: normal;
  font-weight: normal;
  font-size: 14px;
  line-height: 24px;
  /* or 171% */

  color: #3f454a;
}

.article-col-menu {
  animation-delay: 1.5s;
  animation-duration: 2s;
}
.article-col-cards {
  animation-delay: 1.5s;
  animation-duration: 2s;
}
.animate-cards {
  animation-delay: 3.2s;
  animation-duration: 1.5s;
}

.article-row-title {
  animation-delay: 1.5s;
  animation-duration: 2s;
}

.hide {
  visibility: hidden !important;
}
</style>
