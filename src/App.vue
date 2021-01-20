<template>
  <div id="app">
    <md-app md-waterfall md-mode="fixed">
      <md-app-toolbar class="md-primary">
        <div class="md-toolbar-section-start">
          <div class="md-title">MSO Icons lookup</div>
        </div>
        <md-autocomplete md-layout="box" :md-options="[]" v-model="searchTerm">
          <label>Search...</label>
        </md-autocomplete>
        <div class="md-toolbar-section-end">
          <div class="md-body">{{ filteredIcons.length }} icons</div>
        </div>
      </md-app-toolbar>
      <md-app-content>
        <md-empty-state
          v-if="filteredIcons.length == 0"
          md-icon="youtube_searched_for"
          md-label="No icons match your search"
          md-description="Try searching for another term to find icons that match what you're looking for."
        >
        </md-empty-state>
        <Icons v-else :icons="filteredIcons"></Icons>
      </md-app-content>
    </md-app>
  </div>
</template>

<script>
import { ICON_OFFSETS } from "./assets/IconKeys";
import Icons from "./components/Icons.vue";

export default {
  name: "App",
  components: { Icons },
  data() {
    return {
      iconKeys: ICON_OFFSETS,
      searchTerm: "",
    };
  },
  computed: {
    icons() {
      return this.iconLabels.map((label) => {
        return {
          label,
          offset: this.iconKeys[label],
        };
      });
    },
    iconLabels() {
      return Object.keys(this.iconKeys);
    },
    filteredIcons() {
      return this.icons.filter((x) =>
        x.label.toLowerCase().includes(this.searchTerm.toLowerCase())
      );
    },
  },
};
</script>

<style>
::-webkit-scrollbar {
  background: transparent;
  height: 8px;
  width: 8px;
}
::-webkit-scrollbar-thumb {
  border: none;
  -webkit-box-shadow: none;
  box-shadow: none;
  background: #bdc1c6;
  -webkit-border-radius: 8px;
  border-radius: 8px;
  min-height: 40px;
}
:hover::-webkit-scrollbar-thumb {
  background: #dadce0;
}
html,
body,
#app {
  height: 100%;
}
.md-title {
  user-select: none;
}
.md-app {
  max-height: 100vh;
}
.md-autocomplete {
  max-width: 600px;
  margin: 0 auto !important;
}
</style>
