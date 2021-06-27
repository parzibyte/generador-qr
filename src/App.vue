<template>
  <div>
    <b-navbar style="border: 2px solid #ffe08a">
      <template #brand>
        <b-navbar-item tag="a" target="_blank" :href="logo">
          <img :src="logo" style="min-height: 85px" />
        </b-navbar-item>
      </template>
      <template #start>
        <b-navbar-item><strong>Generador de códigos QR</strong></b-navbar-item>
      </template>
      <template #end>
        <b-navbar-item tag="div">
          <div class="buttons">
            <a href="https://parzibyte.me/#contacto" class="button is-danger">
              <b-icon icon="help"></b-icon>
              <strong>Ayuda y soporte</strong>
            </a>
          </div>
        </b-navbar-item>
      </template>
    </b-navbar>
    <section class="p-2">
      <div class="columns">
        <div class="column">
          <b-field
            label="Contenido"
            message="Puede ser una URL, página de Facebook, texto, etcétera"
          >
            <b-input
              @change.native="actualizarCodigoQr()"
              v-model="detallesQr.contenido"
              type="textarea"
            ></b-input>
          </b-field>
          <b-field label="Color">
            <TwitterPicker :value="colors"></TwitterPicker>
          </b-field>
        </div>
        <div class="column is-one-third">
          <div class="card has-text-centered">
            <header class="card-header">
              <p class="card-header-title">Previsualización</p>
              <button class="card-header-icon" aria-label="more options">
                <span class="icon">
                  <i class="fas fa-angle-down" aria-hidden="true"></i>
                </span>
              </button>
            </header>
            <div class="card-content">
              <div class="content">
                <img alt="Código QR generado por la app" id="codigo" />
                <br />
                <b-button type="is-success">Descargar</b-button>
              </div>
            </div>
          </div>
        </div>
      </div>

      <p>
        Lorem ipsum dolor sit amet consectetur, adipisicing elit. Repellendus,
        quas qui cum rerum molestiae animi ad doloribus earum! Iure praesentium,
        atque harum autem quis suscipit et numquam repellat laborum ipsam.
      </p>
    </section>
    <footer class="footer">
      <div class="content has-text-centered">
        <p>
          <strong>Generador de códigos QR </strong>creado y mantenido con
          <b-icon icon="heart" type="is-danger"></b-icon> por
          <a href="https://parzibyte.me/blog">Parzibyte</a>
        </p>
      </div>
    </footer>
  </div>
</template>

<script>
import Logo from "./assets/logo.png";
import QRious from "qrious";
import { Twitter } from "vue-color";
export default {
  components: {
    TwitterPicker: Twitter,
  },
  name: "app",
  data: () => ({
    logo: Logo,
    detallesQr: {
      contenido: "",
    },
    qr: null,
    colors: {
      hex: "#194d33",
      hex8: "#194D33A8",
      hsl: { h: 150, s: 0.5, l: 0.2, a: 1 },
      hsv: { h: 150, s: 0.66, v: 0.3, a: 1 },
      rgba: { r: 25, g: 77, b: 51, a: 1 },
      a: 1,
    },
  }),
  methods: {
    actualizarCodigoQr() {
      console.log("Lo actualizo");
      this.qr.value = this.detallesQr.contenido;
    },
  },
  mounted() {
    this.qr = new QRious({
      element: document.querySelector("#codigo"),
      // value: "https://parzibyte.me/blog",
      // level: "H",
      // size: 300,
      // backgroundAlpha: 0,
    });
    console.log(typeof QRious);
  },
};
</script>