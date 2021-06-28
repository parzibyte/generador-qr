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
              @keyup.native="actualizarCodigoQr()"
              v-model="detallesQr.value"
              ref="textareaContenido"
              type="textarea"
            ></b-input>
          </b-field>
          <b-field grouped>
            <b-field label="Calidad">
              <b-select
                @input="actualizarCodigoQr()"
                placeholder="Seleccione"
                v-model="detallesQr.level"
              >
                <option value="L">Baja</option>
                <option value="M">Media</option>
                <option value="Q">Alta</option>
                <option value="H">Máxima</option>
              </b-select>
            </b-field>
            <b-field
              label="Tamaño de la imagen"
              message="Aunque el tamaño no parezca cambiar, al descargar la imagen tendrá el tamaño seleccionado"
            >
              <b-slider
                v-model="detallesQr.size"
                :min="10"
                @change="actualizarCodigoQr()"
                :max="1000"
              ></b-slider>
            </b-field>
          </b-field>
          <b-field grouped>
            <b-field label="Color del código">
              <SeleccionadorColor
                @input="onColorCambiado"
                :value="colores"
              ></SeleccionadorColor>
            </b-field>

            <b-field label="Color de fondo">
              <SeleccionadorColor
                @input="onColorDeFondoCambiado"
                :value="coloresFondo"
              ></SeleccionadorColor>
            </b-field>
            <b-field label="Opacidad del fondo">
              <b-slider
                v-model="detallesQr.backgroundAlpha"
                :min="0"
                :step="0.1"
                @change="actualizarCodigoQr()"
                :max="1"
              ></b-slider>
            </b-field>
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
                <b-button @click="descargar()" type="is-success"
                  >Descargar</b-button
                >
              </div>
            </div>
          </div>
        </div>
      </div>
      <p>
        Con este generador de códigos QR puedes personalizar el contenido (URL,
        texto, número telefónico, enlace a página de Facebook) del código QR así
        como el tamaño, colores, calidad y transparencia
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
const colorPorDefecto = "#000000",
  colorDeFondoPorDefecto = "#ffffff",
  nivelCorrecionAlto = "H";
import Logo from "./assets/logo.png";
import QRious from "qrious";
import { Sketch } from "vue-color";
export default {
  components: {
    SeleccionadorColor: Sketch,
  },
  name: "app",
  data: () => ({
    logo: Logo,
    detallesQr: {
      value: "",
      foreground: colorPorDefecto,
      background: colorDeFondoPorDefecto,
      size: 200,
      level: nivelCorrecionAlto,
    },
    qr: null,
    colores: {
      hex: colorPorDefecto,
    },
    coloresFondo: {
      hex: colorDeFondoPorDefecto,
    },
  }),
  methods: {
    descargar() {
      const a = document.createElement("a");
      a.href = document.querySelector("#codigo").src;
      a.download = "Código QR.png";
      a.click();
    },
    actualizarCodigoQr() {
      this.qr.set({
        value: this.detallesQr.value,
        foreground: this.detallesQr.foreground,
        background: this.detallesQr.background,
        size: this.detallesQr.size,
        backgroundAlpha: this.detallesQr.backgroundAlpha,
        level: this.detallesQr.level,
      });
    },
    onColorCambiado(nuevoColor) {
      this.detallesQr.foreground = nuevoColor.hex;
      this.actualizarCodigoQr();
    },
    onColorDeFondoCambiado(nuevoColor) {
      this.detallesQr.background = nuevoColor.hex;
      this.actualizarCodigoQr();
    },
  },
  mounted() {
    this.qr = new QRious({
      element: document.querySelector("#codigo"),
    });
    this.actualizarCodigoQr();
    this.$refs.textareaContenido.focus();
  },
};
</script>