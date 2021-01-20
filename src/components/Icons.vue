<template>
  <div id="icons">
    <div
      class="icon md-elevation-1"
      v-for="icon of icons"
      :key="icon.label"
      @mouseenter="addShadow($event)"
      @mouseleave="removeShadow($event)"
      @click="addToClipboard(icon.label, $event)"
    >
      <div
        class="preview"
        :style="{ 'background-position': '0 -' + icon.offset + 'px' }"
      ></div>
      <div class="label">
        {{ icon.label }}
      </div>
    </div>
    <md-snackbar
      md-position="center"
      :md-active.sync="showSnackbar"
      md-persistent
    >
      <span>"{{ clipboard }}" copied to clipboard!</span
      ><md-button class="md-primary" @click="showSnackbar = false"
        >Ok</md-button
      ></md-snackbar
    >
  </div>
</template>

<script>
export default {
  name: "Icons",
  props: {
    icons: Array,
  },
  data() {
    return {
      showSnackbar: false,
      clipboard: null,
    };
  },
  methods: {
    toggleShadow(elem) {
      elem.classList.toggle("md-elevation-3");
    },
    addShadow(e) {
      this.toggleShadow(e.target);
    },
    removeShadow(e) {
      this.toggleShadow(e.target);
    },
    addToClipboard(text, e) {
      const elem = e.target;
      const input = document.createElement("input");
      elem.appendChild(input);
      input.value = text;
      input.select();
      input.setSelectionRange(0, 99999);
      document.execCommand("copy");
      elem.removeChild(input);
      this.showSnackbar = true;
      this.clipboard = text;
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
* {
  user-select: none;
}
#icons {
  display: grid;
  grid-template-columns: repeat(5, 1fr);
  width: 100%;
  max-width: 800px;
  margin: auto;
  gap: 10px;
}
.icon {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 100px;
  padding: 10px;
  box-sizing: border-box;
  background-color: initial;
}
.icon.selected {
  background-color: #448aff;
}
.label {
  font-size: small;
  word-break: break-all;
  text-align: center;
  margin-top: 10px;
}
.preview {
  height: 16px;
  width: 16px;
  background: url("https://drive.google.com/uc?id=12jl45AVuFqZkw0eX2Hz9XQKYOXTXuspj");
  background-repeat: no-repeat;
}
input {
  display: none;
}
</style>
