<template>
  <div 
    class="card" 
    @click="toggleFlip" 
    :class="{ flipped: isFlipped }" 
    @mouseover="handleHover(true)" 
    @mouseleave="handleHover(false)"
  >
    <div class="card-inner">
      <div class="card-front">
        <h2 class="card-title">{{ name }}</h2>
        <span class="card-date">{{ date }}</span>
      </div>

      <div class="card-back">
        <p class="card-description" v-html="description"></p>
        <!-- <a :href="link" target="_blank" rel="noopener noreferrer" class="card-icon">
          <img class="linkIcon" />
        </a> -->
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';

const props = defineProps({
  name: String,
  date: String,
  description: String,
});

const isFlipped = ref(false);
const isTouchDevice = ref(false);

onMounted(() => {
  isTouchDevice.value = 'ontouchstart' in window || navigator.maxTouchPoints > 0;
});

const toggleFlip = () => {
  if (isTouchDevice.value) {
    isFlipped.value = !isFlipped.value;
  }
};

const handleHover = (state) => {
  if (!isTouchDevice.value) {
    isFlipped.value = state;
  }
};
</script>

<style scoped>
/* .linkIcon {
  content: url("/black.png");
}

.dark .linkIcon {
  content: url("/white.png");
} */

.card {
  width: 320px;
  height: 180px;
  perspective: 1000px;
  cursor: pointer;
}

.card-inner {
  width: 100%;
  height: 100%;
  position: relative;
  transition: transform 0.6s;
  transform-style: preserve-3d;
}

.card.flipped .card-inner {
  transform: rotateY(180deg);
}

.card-front,
.card-back {
  width: 100%;
  height: 100%;
  position: absolute;
  backface-visibility: hidden;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  border-radius: 12px;
  transition: box-shadow 0.3s;
}

.card-front {
  background: white;
  border: 1px solid #ddd;
}

.dark .card-front {
  background: #333;
  border: 1px solid #333;
}

.card-back {
  background: white;
  color: white;
  transform: rotateY(180deg);
  text-align: center;
  padding: 16px;
  border: 1px solid #ddd;
}

.dark .card-back {
  background: #333;
  border: 1px solid #333;
}

.card-title {
  font-size: 1.4rem;
  font-weight: bold;
  color: #222222;
}

.dark .card-title {
  color: #F8F9FA;
}

.card-date {
  font-size: 0.9rem;
  color: #555555;
}

.dark .card-date {
  color: #B0B0B0;
}

.card-description {
  font-size: 1rem;
  font-weight: bold;
  color: #333333;
}

.dark .card-description {
  color: #E0E0E0;
}

:deep(.card-description a) {
  text-decoration: underline;
}

/* .card-icon {
  position: absolute;
  bottom: 12px;
  right: 12px;
  display: flex;
  align-items: center;
  justify-content: center;
  width: 28px;
  height: 28px;
  border-radius: 50%;
  background: #007bff;
  color: white;
  transition: background 0.3s ease-in-out;
} */

/* .card-icon:hover {
  background: #0056b3;
} */
</style>