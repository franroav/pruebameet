<template>
  <v-app>
    <!-- 1°first ROW -->
    <div class="row">
      <div class="col-md-12">
        <!-- 1°first Card -->
        <div class="card">
          <div class="card-header text-white bg-secondary">
            Personajes Favoritos
            <v-tooltip top>
              <template v-slot:activator="{ on }">
                <span
                  v-on="on"
                  class="badge badge-dark badge-pill float-right sdw"
                >
                  <i class="fa fa-user text-white"></i>
                </span>
              </template>
              <span>Favorite Characters</span>
            </v-tooltip>
          </div>
          <div v-if="loadTransition" class="alert bluesky-line m-0 mb-0"></div>
          <div class="card-body">
            <v-data-table
              :headers="headers"
              :items="favoriteList"
              :page.sync="page"
              :items-per-page="itemsPerPage"
              :single-select="true"
              show-select
              v-model="selected"
              :search="search"
              :sort-by="'id'"
              :sort-desc="true"
              class="elevation-1 datatable"
            >
              <template v-slot:top>
                <v-card class="mt-1">
                  <v-spacer></v-spacer>
                  <v-toolbar flat color="white">
                    <div class="input-group">
                      <span class="input-group-addon">
                        <i class="fa fa-search mr-2 my-2"></i
                      ></span>

                      <input
                        v-model="search"
                        append-icon="search"
                        label="Búsqueda"
                        type="text"
                        class="form-control form-control-md col-md-8"
                        placeholder="Buscar..."
                      />
                    </div>
                    <!-- <v-text-field
                      class="text-xs-center"
                      v-model="search"
                      append-icon="search"
                      label="Búsqueda"
                    ></v-text-field>-->

                    <v-spacer></v-spacer>

                    <v-divider class="mx-4" inset vertical></v-divider>
                    <v-dialog v-model="dialog" max-width="600px">
                      <!-- 2°second Card -->
                      <v-card>
                        <v-card-title>
                          <span class="headline">{{ formTitle }}</span>
                        </v-card-title>

                        <v-card-text>
                          <v-container>
                            <v-form ref="form" v-model="valid" lazy-validation>
                              <!-- 2°second ROW -->
                              <v-row>
                                <v-col
                                  cols="12"
                                  sm="6"
                                  md="4"
                                  v-if="editedItem.id"
                                >
                                  <v-text-field
                                    v-model="editedItem.id"
                                    name="id"
                                    readonly="readonly"
                                    label="ID"
                                    prepend-icon="vpn_key"
                                    filled
                                  ></v-text-field>
                                </v-col>
                                <v-col cols="12" sm="6" md="6">
                                  <v-text-field
                                    v-model="editedItem.name"
                                    label="name"
                                    :rules="defaultRequiredRules"
                                    prepend-icon="face"
                                  ></v-text-field>
                                </v-col>
                                <v-col cols="12" sm="6" md="6">
                                  <v-text-field
                                    v-model="editedItem.status"
                                    label="status"
                                    :rules="defaultRequiredRules"
                                    prepend-icon="check_circle_outline"
                                  ></v-text-field>
                                </v-col>
                                <v-col cols="12" sm="6" md="6">
                                  <v-text-field
                                    v-model="editedItem.species"
                                    label="species"
                                    :rules="defaultRequiredRules"
                                    prepend-icon="help"
                                  ></v-text-field>
                                </v-col>
                                <v-col cols="12" sm="6" md="6">
                                  <v-text-field
                                    v-model="editedItem.type"
                                    label="type"
                                    prepend-icon="assignment_ind"
                                  ></v-text-field>
                                </v-col>
                                <v-col cols="12" sm="6" md="6">
                                  <v-text-field
                                    v-model="editedItem.gender"
                                    label="gender"
                                    :rules="defaultRequiredRules"
                                    prepend-icon="assignment_ind"
                                  ></v-text-field>
                                </v-col>
                                <v-col cols="12" sm="6" md="6">
                                  <v-text-field
                                    v-model="editedItem.created"
                                    label="fecha"
                                    :rules="defaultRequiredRules"
                                    prepend-icon="date_range"
                                  ></v-text-field>
                                </v-col>
                              </v-row>
                            </v-form>
                          </v-container>
                        </v-card-text>

                        <v-card-actions>
                          <v-spacer></v-spacer>
                          <v-btn color="#FF6347" text @click="close"
                            >Cancelar</v-btn
                          >
                          <v-btn color="#00FF00" text @click="save"
                            >Guardar</v-btn
                          >
                        </v-card-actions>
                      </v-card>
                    </v-dialog>
                  </v-toolbar>
                </v-card>
              </template>
              <template v-slot:item.id="{ item }">
                <v-chip :color="getColor(item.id)" dark>{{ item.id }}</v-chip>
              </template>
              <template v-slot:item.image="{ item }">
                <img :src="item.image" class="img-responsive img-thumbnail" />
              </template>
              <template v-slot:item.actions="{ item }">
                <v-tooltip bottom>
                  <template v-slot:activator="{ on }">
                    <router-link :to="`/inventario/favoritos/${item.id}`">
                      <v-icon v-on="on" small color="#93a2dd" class="mr-2"
                        >mdi-eye</v-icon
                      >
                    </router-link>
                  </template>
                  <span>Ver favorito {{ item.id }}</span>
                </v-tooltip>
                <v-tooltip bottom>
                  <template v-slot:activator="{ on }">
                    <a
                      v-on="on"
                      class="btn btn-xs btn-danger"
                      @click="editItem(item)"
                      ><i class="fa fa-pencil text-white"></i
                    ></a>
                  </template>
                  <span>Agregar {{ item.name }} {{ " | " + item.id }}</span>
                </v-tooltip>
                <v-tooltip bottom>
                  <template v-slot:activator="{ on }">
                    <v-icon
                      v-on="on"
                      color="#93a2dd"
                      small
                      @click="deleteItem(item)"
                      >mdi-delete</v-icon
                    >
                  </template>
                  <span>Eliminar favorito {{ item.id }}</span>
                </v-tooltip>
              </template>
              <template v-slot:no-data>
                <v-btn
                  color="#87CEFA"
                  :loading="loading"
                  @click="reinitialize()"
                  >Cargando ..</v-btn
                >
              </template>
            </v-data-table>
            <div class="text-center pt-2">
              <v-pagination
                v-model="page"
                :total-visible="10"
                :length="pageCount"
                color="#BA68C"
                prev-icon="mdi-menu-left"
                next-icon="mdi-menu-right"
              ></v-pagination>
            </div>
          </div>
        </div>
      </div>
    </div>
  </v-app>
</template>

<script>
//import { SET_BREADCRUMB } from "@/store/breadcrumbs.module";
import axios from "axios";
export default {
  name: "Favorite",
  data: () => ({
    token: "",
    page: 1,
    pageCount: 0,
    itemsPerPage: 10,
    dialog: false,
    valid: true,
    loading: false,
    url: "",
    boolean: false,
    loadTransition: false,
    search: "",
    selected: [],
    favoriteList: [],
    defaultRequiredRules: [(v) => !!v || "Este campo es obligatorio"],
    windowSize: {
      x: 0,
      y: 0,
    },
    dateMsg:
      new Date().toISOString().substr(0, 10) +
      " " +
      new Date().toISOString().substr(11, 5),
    productsByStore: [],
    categories: [],
    editedIndex: -1,
    editedItem: {
      id: 0,
      name: "",
      status: "",
      species: "",
      type: "",
      gender: "",
      location: "",
      image: "",
      episode: "",
      url: "",
      created: "",
    },
    defaultItem: {
      id: 0,
      name: "",
      status: "",
      species: "",
      type: "",
      gender: "",
      location: "",
      image: "",
      episode: "",
      url: "",
      created: "",
    },
  }),
  mounted() {
    if (localStorage.getItem("token") !== null) {
      this.token = localStorage.getItem("token");
      this.loading = true;
      this.fetchFavorites();
      this.fetchCategories();
    }

    /*this.$store.dispatch(SET_BREADCRUMB, [
      { title: "favoritos", route: "/inventario/favoritos" },
    ]);*/
  },
  computed: {
    formTitle() {
      return this.editedIndex === -1 ? "Nuevo Personaje" : "Agregar Personaje";
    },
    headers() {
      return [
        {
          text: "ID",
          align: "start",
          sortable: false,
          value: "id",
        },
        {
          text: "Nombre",
          value: "name",
        },
        {
          text: "Gender",
          value: "gender",
        },
        {
          text: "Species",
          value: "gender",
        },
        {
          text: "Status",
          value: "status",
        },
        {
          text: "Image",
          value: "image",
        },
        {
          text: "Location",
          value: "location.name",
        },
        { text: "Acciones", value: "actions", sortable: false },
      ];
    },
  },

  watch: {
    dialog(val) {
      val || this.close();
    },
  },

  created() {
    this.initialize();
  },
  methods: {
    initialize() {
      if (localStorage.getItem("deleteFavorite") !== null) {
        this.deletedProductMessage();
      }
    },
    editItem(item) {
      //console.log(item);
      this.editedIndex = this.favoriteList.indexOf(item);
      //console.log(this.favoriteList);
      this.editedItem = Object.assign({}, item);

      console.log(Object.assign({}, this.editedItem));

      //  console.log(product);
      this.dialog = true;
    },
    deleteItem(item) {
      var vm = this;
      let deleteFavorite = Object.assign({}, item);

      const confirmacion = confirm(
        `Esta seguro de eliminar el favorito: ${deleteFavorite.name} || ID: ${deleteFavorite.id}?`
      );
      if (confirmacion) {
        vm.axios({
          url: "inventory/products/" + deleteFavorite.id,
          method: "DELETE",
        })
          .then((response) => {
            vm.fetchFavorites();

            vm.$bvToast.toast(
              `El favorito: ${deleteFavorite.name} ha sido eliminado, el ${vm.dateMsg} exitosamente!`,
              {
                title: `Información`,
                variant: "success",
                solid: true,
                toaster: "b-toaster-bottom-center",
              }
            );
          })
          .catch((error) => {
            console.log(error);
          });
      }
    },
    close() {
      this.dialog = false;
      this.$nextTick(() => {
        this.editedItem = Object.assign({}, this.defaultItem);
        this.editedIndex = -1;
      });
    },

    save() {
      var vm = this;

      //console.log(this.$refs.form);

      if (this.editedIndex > -1) {
        //Object.assign(this.favoriteList[this.editedIndex]);

        // EDIT PRODUCT
        let editedFavorite = Object.assign({}, this.editedItem);
        console.log(editedFavorite);
        //console.log(editedFavorite.name);

        /*if (!vm.editedItem.name.length || vm.editedItem.name == null) {
          vm.$bvToast.toast("El nombre del favorito no debe ir vacío", {
            title: `Formulario Invalido`,
            variant: "danger",
            solid: true,
            toaster: "b-toaster-bottom-center",
          });
          vm.$refs.form.$el[1].focus();
          return;
        }

        if (
          vm.editedItem.category_id === 0 ||
          isNaN(vm.editedItem.category_id)
        ) {
          vm.$bvToast.toast("Seleccione una categoría del favorito porfavor", {
            title: `Formulario Invalido`,
            variant: "danger",
            solid: true,
            toaster: "b-toaster-bottom-center",
          });
          vm.$refs.form.$el[2].focus();
          return;
        }

        if (vm.editedItem.price === 0 || isNaN(vm.editedItem.price)) {
          vm.$bvToast.toast("El precio del favorito no debe ir vacío", {
            title: `Formulario Invalido`,
            variant: "danger",
            solid: true,
            toaster: "b-toaster-bottom-center",
          });
          vm.$refs.form.$el[4].focus();
          return;
        }

        if (vm.editedItem.cost === 0 || isNaN(vm.editedItem.cost)) {
          vm.$bvToast.toast("El costo del favorito no debe ir vacío", {
            title: `Formulario Invalido`,
            variant: "danger",
            solid: true,
            toaster: "b-toaster-bottom-center",
          });
          vm.$refs.form.$el[5].focus();
          return;
        }

        if (!vm.editedItem.sku.length || vm.editedItem.sku == null) {
          vm.$bvToast.toast("El SKU del favorito no debe ir vacío", {
            title: `Formulario Invalido`,
            variant: "danger",
            solid: true,
            toaster: "b-toaster-bottom-center",
          });
          vm.$refs.form.$el[6].focus();
          return;
        }

        if (this.$refs.form.validate()) {
          vm.loadTransition = true;
          this.axios({
            url: "inventory/products/" + vm.editedItem.id,
            method: "PUT",
            data: {
              sku: vm.editedItem.sku,
              name: vm.editedItem.name,
              cost: vm.editedItem.cost,
              price: vm.editedItem.price,
              ean: vm.editedItem.ean,
              category_id: vm.editedItem.category_id,
              store_id: vm.store_id,
            },
          })
            .then((response) => {
              vm.fetchFavorites();

              vm.$bvToast.toast(
                `El favorito: ${editedFavorite.name}, ha sido Actualizado el ${vm.dateMsg} exitosamente!`,
                {
                  title: `Información`,
                  variant: "success",
                  solid: true,
                  toaster: "b-toaster-bottom-center",
                }
              );

              vm.loadTransition = false;
              this.$refs.form.reset();
            })
            .catch((error) => {
              console.log(error);
            });
        }
      } else {
        //this.productsByStore.data.push(product);

        // CREATE PRODUCT
        let createProduct = Object.assign({}, this.editedItem);

        if (!vm.editedItem.name.length || vm.editedItem.name == null) {
          vm.$bvToast.toast("El nombre del favorito no debe ir vacío", {
            title: `Formulario Invalido`,
            variant: "danger",
            solid: true,
            toaster: "b-toaster-bottom-center",
          });
          vm.$refs.form.$el[0].focus();
          return;
        }

        if (
          vm.editedItem.category_id === 0 ||
          isNaN(vm.editedItem.category_id)
        ) {
          vm.$bvToast.toast("Seleccione una categoría del favorito porfavor", {
            title: `Formulario Invalido`,
            variant: "danger",
            solid: true,
            toaster: "b-toaster-bottom-center",
          });
          vm.$refs.form.$el[1].focus();
          return;
        }

        if (vm.editedItem.price === 0 || isNaN(vm.editedItem.price)) {
          vm.$bvToast.toast("El precio del favorito no debe ir vacío", {
            title: `Formulario Invalido`,
            variant: "danger",
            solid: true,
            toaster: "b-toaster-bottom-center",
          });
          vm.$refs.form.$el[3].focus();
          return;
        }

        if (vm.editedItem.cost === 0 || isNaN(vm.editedItem.cost)) {
          vm.$bvToast.toast("El costo del favorito no debe ir vacío", {
            title: `Formulario Invalido`,
            variant: "danger",
            solid: true,
            toaster: "b-toaster-bottom-center",
          });
          vm.$refs.form.$el[4].focus();
          return;
        }

        if (!vm.editedItem.sku.length || vm.editedItem.sku == null) {
          vm.$bvToast.toast("El SKU del favorito no debe ir vacío", {
            title: `Formulario Invalido`,
            variant: "danger",
            solid: true,
            toaster: "b-toaster-bottom-center",
          });
          vm.$refs.form.$el[5].focus();
          return;
        }

        if (this.$refs.form.validate()) {

          vm.loadTransition = true;

          this.axios({
            url: "inventory/products",
            method: "POST",
            data: {
              sku: vm.editedItem.sku,
              name: vm.editedItem.name,
              cost: vm.editedItem.cost,
              price: vm.editedItem.price,
              ean: vm.editedItem.ean,
              category_id: vm.editedItem.category_id,
              store_id: vm.store_id,
            },
          })
            .then((response) => {
              vm.fetchFavorites();
              vm.$bvToast.toast(
                `El favorito: ${createProduct.name}, ha sido guardado correctamente!`,
                {
                  title: `Información`,
                  variant: "success",
                  solid: true,
                  toaster: "b-toaster-bottom-center",
                }
              );
              vm.loadTransition = false;
              this.$refs.form.reset();
            })
            .catch((error) => {
              console.log(error);
            });
        }*/
      }
      this.close();
    },
    fetchFavorites() {
      var vm = this;

      try {
        var vm = this;
        const next = this.number;

        axios.get(`http://localhost:3000/character`).then((response) => {
          this.loading = false;
          vm.pageCount = Math.ceil(response.data.length / 10);
          response.data.map((character) => {
            vm.favoriteList.unshift(character);
            console.log(character);
          });
          // vm.favoriteList = response.data;

          //console.log(response.data);
          /* this.image = response.data.image;
            this.name = response.data.name;
            this.species = response.data.species;
            this.status = response.data.status;
            this.gender = response.data.gender;
            this.location = response.data.location.name;
            this.number++;*/
        });
      } catch (error) {
        console.log(error);
      }
      /*this.axios({
        url: "http://localhost:3000/character",
        method: "GET",
      })
        .then((response) => {
          console.log(response.data.data);
          this.loading = false;
          vm.pageCount = Math.ceil(response.data.data.length / 10);
          vm.favoriteList = response.data.data;
        })
        .catch((error) => {
          console.log(error);
        }); */
    },
    fetchCategories() {
      /*var vm = this;
      this.axios({
        url: "inventory/categories",
        method: "GET",
      })
        .then((response) => {
          //console.log(response.data.data);
          vm.categories = response.data.data;
        })
        .catch((error) => {
          console.log(error);
        });*/
    },
    onResize() {
      this.windowSize = { x: window.innerWidth, y: window.innerHeight };
    },
    getColor(id) {
      /*if (price >= 15000) return "#32CD32";
      else if (price >= 10000) return "#D2691E";
      else return "#B22222";*/
      return "#B22222";
    },
    deletedProductMessage() {
      let deletedProduct = JSON.parse(localStorage.getItem("deleteFavorite"));

      this.$bvToast.toast(
        `El favorito: ${deletedProduct.name}, ha sido eliminado, el ${this.dateMsg} exitosamente!`,
        {
          title: `Información`,
          variant: "success",
          solid: true,
          toaster: "b-toaster-bottom-center",
        }
      );
      localStorage.removeItem("deleteFavorite");
    },
    reinitialize() {
      this.favoriteList = [];
      this.loading = true;
      //this.fetchFavorites();
    },
    exportExcel() {
      /*  var vm = this;
      let headers = vm.headers.map((e) => e.text);
      let datos = vm.favoriteList;
      this.axios({
        url: "excel/from_array",
        method: "POST",
        data: {
          data: datos,
          cabeceras: headers,
        },
        responseType: "blob",
      })
        .then((response) => {
          const newBlob = new Blob([response.data], {
            type:
              "application/vnd.openxmlformats-officedocument.spreadsheetml.sheet;",
          });
          const data = window.URL.createObjectURL(newBlob);
          window.open(data, "_blank");
        })
        .catch((error) => {
          console.log(error);
        });*/
    },
  },
};
</script>

<style scoped>
/**TABLE */
.datatable table tbody {
  border-spacing: 2px;
  border-color: #646c9a;
  box-sizing: border-box;
}
/*
  .v-data-table > .v-data-table__wrapper > table {
    border-bottom: 5px solid red;
  }
  
  .datatable table tbody tr:not(:first-child) {
      border-bottom: 1px solid #123ca7 !important;
   }*/
/** TABLE HEADER **/
.datatable table thead tr th {
  color: #646c9a !important;
  font-style: normal;
  font-size: 12px !important;
  border-bottom: 1px ridge #e3f1ff !important;
}
/** TABLE BODY **/
.datatable table tbody tr:first-child {
  color: #6c7293;
  font-size: 12px !important;
}
.datatable table tbody tr > td {
  color: #6c7293;
  font-size: 12px !important;
  border-bottom: 1px ridge #e3f1ff !important;
  /*border-top: 1px ridge #93a2dd !important;*/
}
.datatable table tbody tr:last-child {
  border-bottom: 1px ridge #f0f8ff !important;
}
.theme--light.v-data-table .v-data-footer {
  /**border-top: 1px ridge #F0F8FF !important;*/
}
.datatable table .v-data-footer {
  border-top: 1px ridge #f0f8ff !important;
}
/** PAGINACIÓN **/
.datatable > .v-data-footer .v-icon {
  color: royalblue !important;
}
.datatable > .v-data-footer .v-data-footer__select {
  content: "";
  display: inline-block !important;
} /*
.theme--light.v-pagination .v-pagination__item--active {
  color: #ffffff !important;
  background-color: royalblue !important;
}

.datatable > .theme--light .v-pagination .v-pagination__item {
  color: #ffffff !important;
  background-color: royalblue !important;
}*/
</style>