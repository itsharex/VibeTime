<script setup>
import { ref, watch } from 'vue'

const props = defineProps({
  sound: {
    type: Object,
    required: true
  }
})

const emit = defineEmits(['toggle', 'volume-change'])

const isHovered = ref(false)

function onToggle() {
  emit('toggle', props.sound.id)
}

function onVolumeChange(e) {
  const v = parseFloat(e.target.value)
  emit('volume-change', props.sound.id, v)
}

const iconSvg = (name) => {
  const icons = {
    rain: `<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"><path d="M12 2.69l5.66 5.66a8 8 0 1 1-11.31 0z"/></svg>`,
    thunder: `<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"><path d="M19 16.9A5 5 0 0 0 18 7h-1.26a8 8 0 1 0-11.62 9"/><polyline points="13 11 9 17 15 17 11 23"/></svg>`,
    wind: `<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"><path d="M9.59 4.59A2 2 0 1 1 11 8H2m10.59 11.41A2 2 0 1 0 14 16H2m15.73-8.27A2.5 2.5 0 1 1 19.5 12H2"/></svg>`,
    fire: `<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"><path d="M12 12c2-2.96 0-7-1-8 0 3.038-1.773 4.741-3 6-1.226 1.26-2 3.24-2 5a6 6 0 1 0 12 0c0-1.532-1.056-3.94-2-5-1.786 3-2.791 3-4 2z"/></svg>`,
    ocean: `<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"><path d="M2 6c.6.5 1.2 1 2.5 1C7 7 7 5 9.5 5c2.6 0 2.4 2 5 2 2.5 0 2.5-2 5-2 1.3 0 1.9.5 2.5 1"/><path d="M2 12c.6.5 1.2 1 2.5 1 2.5 0 2.5-2 5-2 2.6 0 2.4 2 5 2 2.5 0 2.5-2 5-2 1.3 0 1.9.5 2.5 1"/><path d="M2 18c.6.5 1.2 1 2.5 1 2.5 0 2.5-2 5-2 2.6 0 2.4 2 5 2 2.5 0 2.5-2 5-2 1.3 0 1.9.5 2.5 1"/></svg>`,
    birds: `<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"><path d="M16 7a4 4 0 1 1-8 0"/><path d="M12 11v4"/><path d="M8 18a4 4 0 0 1 8 0"/><line x1="12" y1="2" x2="12" y2="4"/></svg>`,
    whiteNoise: `<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"><path d="M2 12h2l3-9 4 18 4-18 3 9h2"/></svg>`,
    cafe: `<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"><path d="M18 8h1a4 4 0 0 1 0 8h-1"/><path d="M2 8h16v9a4 4 0 0 1-4 4H6a4 4 0 0 1-4-4V8z"/><line x1="6" y1="1" x2="6" y2="4"/><line x1="10" y1="1" x2="10" y2="4"/><line x1="14" y1="1" x2="14" y2="4"/></svg>`,
    forest: `<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"><path d="M17 14l-5-10-5 10"/><path d="M7 14l-3 6h16l-3-6"/><path d="M12 4v2"/></svg>`,
    singingBowl: `<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"><circle cx="12" cy="12" r="10"/><path d="M12 6v6l4 2"/><circle cx="12" cy="12" r="2"/></svg>`,
    fan: `<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"><circle cx="12" cy="12" r="3"/><path d="M12 1v6m0 6v6m-7.5-3l5.2-3m4.6-2.5L19.5 7M1 12h6m6 0h6m-3 7.5l-3-5.2M9.5 11.1L4.5 4.5"/></svg>`,
    piano: `<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"><rect x="2" y="4" width="20" height="16" rx="2"/><path d="M6 4v16m4-16v16m4-16v16m4-16v16"/><path d="M8 4v8m4-8v8m4-8v8" fill="currentColor"/></svg>`
  }
  return icons[name] || icons.whiteNoise
}
</script>

<template>
  <div
    class="sound-card"
    :class="{ active: sound.playing, hovered: isHovered }"
    @mouseenter="isHovered = true"
    @mouseleave="isHovered = false"
    @click="onToggle"
  >
    <div class="sound-icon" v-html="iconSvg(sound.iconKey)"></div>
    <div class="sound-name">{{ sound.name }}</div>
    <div class="sound-desc">{{ sound.description }}</div>
    <div class="volume-control" @click.stop>
      <input
        type="range"
        min="0"
        max="1"
        step="0.01"
        :value="sound.volume"
        @input="onVolumeChange"
        class="volume-slider"
        :class="{ visible: sound.playing || isHovered }"
      />
    </div>
    <div class="volume-label" v-if="sound.playing">
      {{ Math.round(sound.volume * 100) }}%
    </div>
  </div>
</template>

<style scoped>
.sound-card {
  background: rgba(255, 255, 255, 0.04);
  border: 1px solid rgba(255, 255, 255, 0.06);
  border-radius: 16px;
  padding: 20px 16px 16px;
  cursor: pointer;
  transition: all 0.3s ease;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 8px;
  user-select: none;
  min-height: 160px;
}

.sound-card:hover {
  background: rgba(255, 255, 255, 0.08);
  border-color: rgba(255, 255, 255, 0.12);
  transform: translateY(-2px);
}

.sound-card.active {
  background: rgba(255, 255, 255, 0.1);
  border-color: rgba(255, 255, 255, 0.2);
  box-shadow: 0 4px 20px rgba(255, 255, 255, 0.05);
}

.sound-icon {
  width: 36px;
  height: 36px;
  color: rgba(255, 255, 255, 0.5);
  transition: color 0.3s ease;
}

.sound-card.active .sound-icon {
  color: rgba(255, 255, 255, 0.9);
}

.sound-card:hover .sound-icon {
  color: rgba(255, 255, 255, 0.7);
}

.sound-icon :deep(svg) {
  width: 100%;
  height: 100%;
}

.sound-name {
  font-size: 14px;
  font-weight: 600;
  color: rgba(255, 255, 255, 0.8);
  letter-spacing: 0.3px;
}

.sound-card.active .sound-name {
  color: rgba(255, 255, 255, 1);
}

.sound-desc {
  font-size: 11px;
  color: rgba(255, 255, 255, 0.35);
  line-height: 1.3;
}

.volume-control {
  width: 100%;
  margin-top: 4px;
}

.volume-slider {
  width: 100%;
  height: 4px;
  -webkit-appearance: none;
  appearance: none;
  background: rgba(255, 255, 255, 0.15);
  border-radius: 2px;
  outline: none;
  opacity: 0;
  transition: opacity 0.3s ease;
  cursor: pointer;
}

.volume-slider.visible {
  opacity: 1;
}

.volume-slider::-webkit-slider-thumb {
  -webkit-appearance: none;
  appearance: none;
  width: 14px;
  height: 14px;
  border-radius: 50%;
  background: white;
  cursor: pointer;
  box-shadow: 0 1px 4px rgba(0, 0, 0, 0.3);
}

.volume-slider::-moz-range-thumb {
  width: 14px;
  height: 14px;
  border-radius: 50%;
  background: white;
  cursor: pointer;
  border: none;
  box-shadow: 0 1px 4px rgba(0, 0, 0, 0.3);
}

.volume-label {
  font-size: 11px;
  color: rgba(255, 255, 255, 0.4);
  font-variant-numeric: tabular-nums;
}
</style>
