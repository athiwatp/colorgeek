<template>
  <div
    v-if="!editMode"
    :class="['palette-item__color', {'palette-item__color--big': isBig}]"
    :style="{backgroundColor: '#' + color.value}"
    :data-clipboard-text="colorType === 'hex' ? '#' + color.value : rgbColor"
  ></div>
  <div v-else
    :class="['color-picker', {'color-picker--big': isBig}]"
    :style="{backgroundColor: '#' + color.value}">
    <label class="color-picker__label">
      <input class="color-picker__input" type="color" :value="color.value" @input="updateColor($event.target.value)">
    </label>
  </div>
</template>

<script>
import { mapActions } from 'vuex';
import { hexToRgb } from '../helpers';

export default {
  name: 'Color',
  props: {
    editMode: {
      type: Boolean,
      required: true,
    },
    color: {
      type: Object,
      required: true,
    },
    isBig: {
      type: Boolean,
      default: false,
    },
  },
  computed: {
    colorType() {
      return this.$store.state.colorType;
    },
    rgbColor() {
      return hexToRgb(this.color.value);
    },
  },
  methods: {
    ...mapActions({
      updateUserPaletteColor: 'updateUserPaletteColor',
      updateNewPaletteColor: 'updateNewPaletteColor',
    }),
    updateColor(value) {
      const color = { ...this.color, value: value.slice(1) };
      if (this.$route.path.includes('/create')) {
        this.updateNewPaletteColor(color);
      } else {
        this.updateUserPaletteColor(color);
      }
    },
  },
};
</script>

<style scoped>
.palette-item__color, .color-picker {
  height: 100px;
  width: 100px;
  border-radius: 10%;
  margin: 10px;
  border: 2px solid #ccc;
  cursor: url('data:;base64,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'), pointer;
}

.palette-item__color--big, .color-picker--big {
  width: 160px;
}

.palette-item__color:hover {
  border: 2px solid #d55;
}

.color-picker {
  cursor: pointer;
  display: flex;
}

.color-picker__input {
  visibility: hidden;
  width: 0;
  height: 0;
  position: absolute;
  top: 0;
  left: 0;
}

.color-picker__label {
  cursor: pointer;
  position: relative;
  flex: 1 1 0px;
}

@media (max-width: 400px) {
  .palette-item__color, .color-picker {
    height: 80px;
    width: 80px;
  }
  .palette-item__color--big, .color-picker--big {
    width: 130px;
  }
}
</style>
