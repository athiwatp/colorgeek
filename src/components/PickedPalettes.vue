<template>
  <div class="wrapper">
    <div
      class="update-message"
      v-if="palettesWasAdded">
      {{ palettesWasAdded }} new palettes was added. Do you want to see new palettes?
      <span
        @click="loadPickedPalettes(palettesWasAdded)"
        class="update-message__button"
      >Update palettes</span>
    </div>
    <palette-container
      :user="user"
      :palettes="palettes"
      :isPublic="true"
    ></palette-container>
  </div>
</template>

<script>
import { mapActions, mapState } from 'vuex';
import PaletteContainer from './PaletteContainer';

/* eslint-disable no-undef */
export default {
  name: 'PublicPalettes',
  components: {
    PaletteContainer,
  },
  created() {
    this.loadPickedPalettes(12);
    window.addEventListener('scroll', this.handleScroll);
  },
  destroyed() {
    this.clearPublicPalettes();
    window.removeEventListener('scroll', this.handleScroll);
  },
  updated() {
    if (this.isNoMorePalettes) {
      window.removeEventListener('scroll', this.handleScroll);
    }
  },
  computed: {
    user() {
      return this.$store.state.currentUser;
    },
    ...mapState({
      palettes: state => state.public.palettes,
      palettesWasAdded: state => state.public.palettesWasAdded,
      isNoMorePalettes: state => state.public.isNoMorePalettes,
    }),
  },
  methods: {
    ...mapActions({
      loadPickedPalettes: 'loadPickedPalettes',
      clearPublicPalettes: 'clearPublicPalettes',
      addPickedPalettesToEnd: 'addPickedPalettesToEnd',
    }),
    handleScroll() {
      if ((window.innerHeight + window.pageYOffset) >= document.body.offsetHeight
          && this.palettes[this.palettes.length - 1]) {
        this.addPickedPalettesToEnd({
          uid: this.user.uid,
          endKey: this.palettes[this.palettes.length - 1].key,
          palettesNum: 7,
        });
      }
    },
  },
};
</script>

<style scoped>
.update-message {
  text-align: center;
}
.update-message__button {
  cursor: pointer;
  color: #555599;
  text-decoration: underline;
}
</style>
