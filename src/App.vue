<template>
  <div class="app">
    <h1 class="">Random emoji generator 🖊</h1>

    <input @focus="disableSelect" ref="input" class="emoji-input" type="text" v-model="emoji" />

    <div>
      <button @click="generateEmoji" class="btn btn--primary">Generate</button>
      <button @click="copyToClipboard" class="btn btn--secondary">Copy</button>
    </div>
  </div>
</template>

<script>
import 'reset-css';
import 'utilities-css/dist/utilities-css.css';
import { random } from 'lodash';

import './assets/scss/main.scss';
import emojiList from './emoji.json';

export default {
  name: 'App',

  data() {
    return {
      emoji: '😎',
      selectedCategories: [
        'Objects',
        'Smileys & Emotion',
        'Activities',
        'People & Body',
        'Animals & Nature',
        'Food & Drink',
        'Travel & Places',
      ],
    };
  },

  computed: {
    onlyEmoji() {
      return emojiList.map((item) => item.emoji);
    },

    availableCategories() {
      const set = new Set();
      emojiList.forEach((item) => set.add(item.category));
      return [...set];
    },

    emojisFromSelectedCategories() {
      return emojiList
        .filter((item) => this.selectedCategories.includes(item.category))
        .map((item) => item.emoji);
    },
  },

  methods: {
    randomEmoji() {
      return this.onlyEmoji[random(this.onlyEmoji.length)];
    },

    generateEmoji() {
      const r = random(this.emojisFromSelectedCategories.length);
      this.emoji = this.emojisFromSelectedCategories[r];
    },

    copyToClipboard() {
      this.$refs.input.select();
      this.$refs.input.setSelectionRange(0, 32);
      document.execCommand('copy');
    },

    disableSelect(e) {
      window.requestAnimationFrame(e.target.blur.bind(e.target));
    },
  },

  async created() {
    this.generateEmoji();

    console.log(this.availableCategories);
  },
};
</script>
