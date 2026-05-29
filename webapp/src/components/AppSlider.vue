<script setup lang="ts">
import { ref, onMounted, onUnmounted } from 'vue'

const definitions = [
  {
    pos: 'informal',
    text: 'used to denote female control of something regarded as more commonly dominated by men.'
  },
  {
    pos: 'usage note',
    text: 'a humorous or mildly disparaging name for a woman.'
  },
  {
    pos: 'mod.',
    text: 'of, relating to, or controlled by women; female; feminine.'
  }
]

const activeIndex = ref(0)
let intervalId: ReturnType<typeof setInterval> | undefined

onMounted(() => {
  intervalId = setInterval(() => {
    activeIndex.value = (activeIndex.value + 1) % definitions.length
  }, 5000)
})

onUnmounted(() => {
  if (intervalId) {
    clearInterval(intervalId)
  }
})
</script>

<template>
  <div class="tagline">
    <div class="definitions">
      <p
        v-for="(definition, index) in definitions"
        :key="definition.pos"
        class="entry"
        :class="{ focus: index === activeIndex }"
      >
        <strong class="headword">pet·ti·coat</strong>
        <span class="pronunciation">/ˈpɛtɪkəʊt/</span>
        <em class="pos">{{ definition.pos }}</em
        ><span class="def">: {{ definition.text }}</span>
      </p>
    </div>
  </div>
</template>

<style lang="scss" scoped>
.tagline {
  position: fixed;
  bottom: 45px;
  right: 20px;
  z-index: 1;
  width: 375px;
  max-width: calc(100vw - 40px);
  padding: 12px 16px;
  color: var(--color-accent-300);
  font-family: Merriweather, Georgia, serif;
  font-size: 14px;
  line-height: 1.55;
  pointer-events: none;
  background: rgba(0, 0, 0, 0.4);
  border: 1px solid #000;
  backdrop-filter: blur(8px);
  -webkit-backdrop-filter: blur(8px);

  .definitions {
    min-height: 4.5em;
    position: relative;

    .entry {
      position: absolute;
      width: 100%;
      top: 0;
      left: 0;
      margin: 0;
      padding-left: 1.25em;
      text-indent: -1.25em;
      text-align: left;
      opacity: 0;
      transition: opacity 0.8s ease-out;

      &.focus {
        opacity: 1;
        transition: opacity 1.4s ease-in;
      }
    }

    .headword {
      font-weight: 700;
    }

    .pronunciation {
      margin: 0 0.35em;
      font-weight: 400;
    }

    .pos {
      font-style: italic;
      font-weight: 400;
    }

    .def {
      font-weight: 400;
    }
  }
}

@media (max-width: 667px) and (orientation: landscape) {
  .tagline {
    bottom: 10px;
    right: 10px;
    width: auto;
    max-width: 50%;
    font-size: 12px;
  }
}
</style>
