<template>
  <div v-if="this.item" class="detalles-item">
    <div class="container-fluid">
      <div class="row">
        <div class="col-md-12 border-style">
          <h3 class="text-center font-grand-title">
            {{ this.item.name_item }}
          </h3>
        </div>
      </div>
      <div class="row">
        <div class="col-md-6 diseño-grid">
          <h2>
            Descripcion
          </h2>
          <p>
            {{ this.item.description }}
          </p>
        </div>
        <div class="row">
          <div class="col-md-6 diseño-grid">
            <h2>
              Precio
            </h2>
            <p id="priceDetail">
              {{ item.price }} € <br />
              {{ getPercent(item.price, item.price_reduccion.reduced_percent) }}
            </p>
          </div>
          <div class="col-md-6 diseño-grid">
            <h2>
              Estado
            </h2>
            <p>{{ getState(item.state) }}</p>
          </div>
          <div class="w-100"></div>
          <div class="col-md-6 diseño-grid">
            <h2>
              Proveedor
            </h2>
            <p>
              {{ this.item.supplier.name_supplier }}
            </p>
          </div>
        </div>
      </div>
      <div class="row">
        <div class="col-md-4 diseño-grid">
          <h2>
            Fecha de creación
          </h2>
          <p>
            {{ moment(this.item.creation_date).format("YYYY-MM-DD") }}
          </p>
        </div>
        <div class="col-md-4 diseño-grid">
          <h2>
            Usuario del creador
          </h2>
          <p>
            {{ this.item.creation_user.username }}
          </p>
        </div>
        <div class="col-md-4 diseño-grid">
          <router-link class="btn btn-edit" :to="`/items/edit/${item.id_item}`"
            >Editar</router-link
          >
          <button v-on:click="returnList" class="btn  btn-return">
            Volver
          </button>
        </div>
      </div>
    </div>
  </div>
</template>
<style>
.border-style {
  border-block-style: groove;
}

.btn-edit {
  background: #fffb03 !important;
  border-color: black !important;
  color: black !important;
  top: 34%;
  width: 100%;
}
.btn-return {
  background: #0fb900 !important;
  border-color: black !important;
  color: black !important;
  top: 34%;
  width: 100%;
}

.img-principal {
  width: 50%;
  height: 90%;
}
.diseño-grid {
  font-size: 10px;
}
.font-grand-title {
  font-family: "DS-Title", cursive;
  font-size: 80px;
}

@font-face {
  font-family: "DS-Title";
  src: url("../../assets/fonts/OptimusPrinceps.ttf");
}
.detalles-item {
  color: white;
}
@font-face {
  font-family: "DS-Title";
  src: url("../../assets/fonts/OptimusPrinceps.ttf");
}
</style>
<script>
import http from "../../http-common";

export default {
  name: "items-details",
  data() {
    return {
      item: []
    };
  },
  methods: {
    /* eslint-disable no-console */
    retrieveItem() {
      var ide = this.$route.params.id;
      http
        .get("/items/view/" + ide)
        .then(response => {
          this.item = response.data;
        })
        .catch(e => {
          console.log(e);
        });
    },
    redirectEdit(id) {
      window.location.href = "#/item/Edit/" + id;
    },
    returnList() {
      window.location.href = "#/items";
    },
    /* eslint-enable no-console */
    getState(estado) {
      if (estado == 1) {
        return "Activo";
      }
      return "Inactivo";
    },
    getPercent(precio, reduccion) {
      if (reduccion == 0) {
        return "";
      } else {
        return (
          precio -
          (reduccion * precio) / 100 +
          "€" +
          " (Reducido: -" +
          reduccion +
          "%)"
        );
      }
    }
  },
  mounted() {
    this.retrieveItem();
  }
};
</script>
