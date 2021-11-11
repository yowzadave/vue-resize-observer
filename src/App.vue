<template>
  <div id="app">
    <div class="container">
      <div
        ref="a"
        class="viewport"
        v-if="maximized === 'a' || !maximized"
        :class="{ maximized: maximized === 'a'}"
      >
        <div>
          Width: {{ awidth }}
        </div>
        <button v-if="!maximized" @click="maximize('a')">Maximize</button>
        <button v-else @click="minimize">Minimize</button>
      </div>
      <div
        ref="b"
        class="viewport"
        v-if="maximized === 'b' || !maximized"
        :class="{ maximized: maximized === 'b'}"
      >
        <div>
          Width: {{ bwidth }}
        </div>
        <button v-if="!maximized" @click="maximize('b')">Maximize</button>
        <button v-else @click="minimize">Minimize</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      maximized: null,
      awidth: 0,
      bwidth: 0,
    };
  },
  methods: {
    maximize(viewport) {
      this.maximized = viewport;
    },
    minimize() {
      this.maximized = null;
    },
  },
  async mounted() {
    await this.$nextTick();

    this.awidth = this.$refs.a.clientWidth;
    this.bwidth = this.$refs.b.clientWidth;

    new ResizeObserver((entities) => {
      this.awidth = entities[0].target.clientWidth;
    }).observe(this.$refs.a);

    new ResizeObserver((entities) => {
      this.bwidth = entities[0].target.clientWidth;
    }).observe(this.$refs.b);
  },
};
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

.container {
  display: flex;
}

.viewport {
  width: 300px;
  height: 300px;
  border: 1px solid red;

  &.maximized {
    width: 600px;
  }
}
</style>
