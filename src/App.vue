<template>
  <Glossaries :words="words" @wordsChange="wordsChange" />
  <select v-model="selectedVoiceIndex">
    <option v-for="(voice, index) in voices" :value="index">
      {{ voice.name }}
    </option>
  </select>
  <button @click="play" v-if="!isPlaying">Play</button>
  <button @click="stop" v-if="isPlaying">Stop</button>
</template>

<script>
import Glossaries from "./components/Glossaries.vue";

const synth = window.speechSynthesis;

export default {
  name: "App",
  components: { Glossaries },

  data: () => {
    return {
      words: ["Äpple", "Banan", "Citron"],
      voices: [],
      selectedVoiceIndex: 0, // voice.default
      isPlaying: false,
      speakTimeout: null,
    };
  },

  methods: {
    wordsChange(words) {
      this.words = words;
    },

    play() {
      this.isPlaying = true;
      this.speak(0);
    },

    speak(wordIndex) {
      console.log("SPEAK", wordIndex, this.words[wordIndex]);
      this.say(this.words[wordIndex]);
      wordIndex++;
      if (this.words.length > wordIndex) {
        this.speakTimeout = setTimeout(() => {
          this.speak(wordIndex);
        }, 3000);
      } else {
        this.isPlaying = false;
      }
    },

    stop() {
      this.isPlaying = false;
      clearTimeout(this.speakTimeout);
      this.speakTimeout = null;
    },

    say(text) {
      const utterThis = new SpeechSynthesisUtterance(text);
      utterThis.voice = this.voices[this.selectedVoiceIndex];

      // utterThis.pitch = pitch.value;
      // utterThis.rate = rate.value;
      synth.speak(utterThis);
    },
  },

  mounted() {
    setTimeout(() => {
      console.log(window.speechSynthesis.getVoices());
      this.voices = window.speechSynthesis.getVoices();
    }, 1);

    const defaultVoiceIndex = this.voices.findIndex(
      (voice) => voice.default === true
    );

    if (defaultVoiceIndex) {
      this.selectedVoiceIndex = defaultVoiceIndex;
    }
  },
};
</script>
