<template>
  <div id="app">
    <div class="song-info">
      <h2>{{ songTitle }}</h2>
      <p>{{ songComposer }}</p>
    </div>
    <div class="lyrics-container-wrapper">
      <div class="lyrics-container" ref="lyricsContainer">
        <div v-for="(line, index) in lyrics" :key="index" :class="{
          'highlighted': index === currentIndex,
          'passed': index < currentIndex
        }" class="lyric-line" @click="highlightLyric(index)" :ref="'lyric' + index">
          {{ line }}
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      isPresenting: false,
      songTitle: '',
      songComposer: '',
      lyrics: [],
      currentIndex: 0,
      songs: ["song_1.json", "song_2.json", "song_3.json"],
      currentSongIndex: 0
    };
  },
  watch: {
    currentIndex(newIndex) {
      this.scrollToLyric(newIndex);
    }
  },
  created() {
    this.loadLyrics(this.songs[this.currentSongIndex]);
    window.addEventListener("keydown", this.handleKeyDown);
  },
  beforeUnmount() {
    window.removeEventListener("keydown", this.handleKeyDown);
  },
  methods: {
    async loadLyrics(song) {
      const response = await fetch(`./assets/${song}`);
      const data = await response.json();
      this.lyrics = data.lyrics;
      this.songTitle = data.title;
      this.songComposer = data.composer;
      this.currentIndex = 0; // Reset current index when loading a new song
    },
    handleKeyDown(event) {
      if (event.code === "Space") {
        this.currentIndex = (this.currentIndex + 1) % this.lyrics.length;
      } else if (event.code === "ArrowRight") {
        this.currentSongIndex = (this.currentSongIndex + 1) % this.songs.length;
        this.loadLyrics(this.songs[this.currentSongIndex]);
      } else if (event.code === "ArrowLeft") {
        this.currentSongIndex =
          (this.currentSongIndex - 1 + this.songs.length) % this.songs.length;
        this.loadLyrics(this.songs[this.currentSongIndex]);
      } else if (event.code === "ArrowUp") {
        this.currentIndex = (this.currentIndex - 1 + this.lyrics.length) % this.lyrics.length;
      } else if (event.code === "ArrowDown") {
        this.currentIndex = (this.currentIndex + 1) % this.lyrics.length;
      } else if (event.code === "F5") {
        this.isPresenting = !this.isPresenting;
      }
    },
    created() {
      this.loadLyrics(this.songs[this.currentSongIndex]);
      window.addEventListener("keydown", this.handleKeyDown);
    },
    beforeUnmount() {
      window.removeEventListener("keydown", this.handleKeyDown);
    },

    highlightLyric(index) {
      this.currentIndex = index;
    },
    scrollToLyric(index) {
      const lyricElement = this.$refs['lyric' + index];
      const lyricsContainer = this.$refs.lyricsContainer;
      if (lyricElement && lyricElement.length && lyricsContainer) {
        const elementTop = lyricElement[0].offsetTop;
        const containerHeight = lyricsContainer.clientHeight;
        lyricsContainer.scrollTo({
          top: elementTop - containerHeight / 2,
          behavior: 'smooth'
        });
      }
    }
  }
};
</script>

<style>
body {
  margin: 0;
  background-color: #ecf0f1;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  color: #2c3e50;
  display: flex;
  justify-content: space-between;
  padding: 20px;
}

.song-info {
  width: 25vw;
  text-align: left;
}

.song-info h2 {
  font-size: 2.5rem;
}

.song-info p {
  font-size: 1.5rem;
}

.lyrics-container-wrapper {
  width: 60vw;
  max-height: 80vh;
  overflow: hidden;
  background-color: rgb(6, 18, 110);
  padding: 2rem;
  border-radius: 2rem;
  position: relative;
}

.lyrics-container {
  max-height: 100%;
  overflow-y: scroll;
  scroll-behavior: smooth;
  padding-right: 16px;
  /* Adjust to create space for the scrollbar */
  box-sizing: content-box;
  scrollbar-width: none;
  /* Hide scrollbar for Firefox */
}

.lyrics-container:hover {
  scrollbar-width: thin;
  scrollbar-color: rgba(0, 0, 0, 0.5) transparent;
  /* Firefox */
}

/* For WebKit browsers (Chrome, Safari) */
.lyrics-container::-webkit-scrollbar {
  width: 0;
}

.lyrics-container:hover::-webkit-scrollbar {
  width: 8px;
  /* Show scrollbar on hover */
}

.lyrics-container::-webkit-scrollbar-track {
  background: transparent;
}

.lyrics-container::-webkit-scrollbar-thumb {
  background-color: rgba(0, 0, 0, 0.5);
  border-radius: 10px;
  border: 2px solid transparent;
  background-clip: content-box;
}

.lyrics-container:hover::-webkit-scrollbar-thumb {
  background-color: rgba(0, 0, 0, 0.8);
}

.lyric-line {
  color: rgba(243, 160, 70, 0.75);
  margin: 5px 0;
  font-size: 4rem;
  cursor: pointer;
  font-weight: 900;
}

.lyric-line.highlighted {
  color: rgb(228, 200, 18);
}

.lyric-line.passed {
  color: rgb(242, 248, 165);
}
</style>
