<template>
  <div :class="classList">
    <div ref="body" class="theme__body">
      <slot />
    </div>
    <transition :name="blockSelector" @after-enter="handleSwitch">
      <div ref="switcher" :class="switcherClassList" v-show="switching"></div>
    </transition>
  </div>
</template>

<script>
import { computed } from "vue";

const BLOCK_SELECTOR = "theme";
const DEFAULT_THEME = "default";

export default {
  name: "ThemeLayer",
  data() {
    return {
      switching: false,
      switchTheme: DEFAULT_THEME,
      blockSelector: BLOCK_SELECTOR,
      theme: DEFAULT_THEME
    };
  },
  provide() {
    return {
      currentTheme: computed(() => this.theme),
      changeTheme: theme => {
        this.prepare(theme);
      }
    };
  },
  computed: {
    classList() {
      return [BLOCK_SELECTOR, `${BLOCK_SELECTOR}--${this.theme}`];
    },
    switcherClassList() {
      const blockSelector = `${BLOCK_SELECTOR}__switcher`;

      return [blockSelector, `${blockSelector}--${this.switchTheme}`];
    }
  },
  methods: {
    prepare(nextTheme) {
      this.$refs.switcher.innerHTML = ""; // reset switcher content
      this.$refs.switcher.innerHTML = this.$refs.body.outerHTML; // get body HTML inside switcher
      this.switchTheme = nextTheme;
      this.switching = true;

      setTimeout(() => {
        this.$refs.switcher.scrollTo(0, window.pageYOffset);
      }, 0);
    },
    handleSwitch() {
      this.theme = this.switchTheme;
      this.switching = false;
      this.$refs.switcher.innerHTML = ""; // reset switcher content
    }
  }
};
</script>

<style lang="scss" scoped>
.theme {
  height: 100%;

  &,
  &__switcher {
    &--default {
      --theme-background-color: white;
      --theme-text-color: black;
    }

    &--dark {
      --theme-background-color: black;
      --theme-text-color: white;
    }

    &--blue {
      --theme-background-color: white;
      --theme-text-color: blue;
    }
  }

  &__body {
    height: 100%;
    background-color: var(--theme-background-color);
    color: var(--theme-text-color);
  }

  &__switcher {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    z-index: 100;
    overflow-y: auto;
    overflow-x: hidden;
  }

  &-enter-active,
  &-leave-active {
    transition: clip-path 300ms;
  }

  &-enter-from,
  &-leave-to {
    clip-path: polygon(-40% 100%, -20% 0, -20% 0, -40% 100%);
  }

  &-enter-to,
  &-leave-from {
    clip-path: polygon(-40% 100%, -20% 0, 120% 0, 100% 100%);
  }
}
</style>
