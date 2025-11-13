<script setup>
import '../assets/home.css'
import { onMounted, ref, onBeforeUnmount } from 'vue'
import { motion, useAnimate } from 'motion-v'

const [scope, animate] = useAnimate()

// Create baseball rain
const baseballs = ref([])
const createBaseballs = () => {
  for (let i = 0; i < 15; i++) {
    baseballs.value.push({
      id: i,
      left: i * 5 + Math.random() * 3,
      delay: Math.random() * 5,
      duration: 3 + Math.random() * 2,
    })
  }
}

// catch the ball

let frameId

const checkCollisions = () => {
  const container = scope.value
  if (!container) {
    frameId = requestAnimationFrame(checkCollisions)
    return
  }

  const trash = container.querySelector('.trash-can')
  if (!trash) {
    frameId = requestAnimationFrame(checkCollisions)
    return
  }

  const trashRect = trash.getBoundingClientRect()
  const balls = container.querySelectorAll('.baseball')

  balls.forEach((ball) => {
    // already collected, skip
    if (ball.dataset.collected === '1') return

    const rect = ball.getBoundingClientRect()

    const overlap =
      rect.bottom > trashRect.top &&
      rect.top < trashRect.bottom &&
      rect.right > trashRect.left &&
      rect.left < trashRect.right

    if (overlap) {
      // mark as collected
      ball.dataset.collected = '1'
      ball.classList.add('collected')

      // animate ball into nothing
      animate(ball, { scale: 0, opacity: 0 }, { duration: 0.25 })
    }
  })

  frameId = requestAnimationFrame(checkCollisions)
}

onMounted(() => {
  createBaseballs()

  animate(
    '.splash-head',
    { opacity: 1, y: 0 },
    { duration: 0.8, type: 'spring', stiffness: 100, damping: 10 },
  ).then(() => {
    animate('.splash-subhead', { opacity: 1 }, { duration: 0.6 })
    animate('.buttons', { opacity: 1 }, { duration: 0.6, delay: 0.2 })
  })
  frameId = requestAnimationFrame(checkCollisions)
})
</script>

<template>
  <div class="splash-container" ref="scope">
    <!-- Baseball rain -->
    <motion.div
      v-for="ball in baseballs"
      :key="ball.id"
      class="baseball"
      :style="{ left: ball.left + '%' }"
      :initial="{ y: -100 }"
      :animate="{ y: 'calc(100vh + 100px)' }"
      :transition="{
        duration: ball.duration,
        delay: ball.delay,
        repeat: Infinity,
        ease: 'linear',
      }"
    >
      ⚾
    </motion.div>

    <div class="splash">
      <h1 class="splash-head" style="opacity: 0; transform: translateY(-50px)">
        Should the Minnesota Twins keep Pablo Lopez?
      </h1>

      <p class="splash-subhead" style="opacity: 0">
        Consent to this survey and help out fellow Twins fans
      </p>

      <p class="buttons" style="opacity: 0">
        <motion.span :whileHover="{ scale: 1.05 }" :whileTap="{ scale: 0.95 }">
          <RouterLink to="/survey" class="btn btn-consent">Consent</RouterLink>
        </motion.span>

        <motion.span :whileHover="{ scale: 1.05 }" :whileTap="{ scale: 0.95 }">
          <RouterLink to="/decline" class="btn btn-decline">Decline</RouterLink>
        </motion.span>
      </p>
    </div>

    <!-- Draggable trash can -->
    <motion.div
      class="trash-can"
      :drag="true"
      :dragTransition="{ bounceStiffness: 200, bounceDamping: 15 }"
    >
      🗑️
    </motion.div>
    <!-- end of trash can -->
  </div>
</template>

<style scoped>
.baseball {
  position: absolute;
  font-size: 30px;
  pointer-events: none;
  z-index: 5;
  top: 0;
  filter: drop-shadow(0 0 2px rgba(255, 255, 255, 0.25));
  animation: glowBall 4s ease-in-out infinite;
}

@keyframes glowBall {
  0% {
    filter: drop-shadow(0 0 2px rgba(255, 255, 255, 0.2));
  }
  50% {
    filter: drop-shadow(0 0 4px rgba(255, 255, 255, 0.35));
  }
  100% {
    filter: drop-shadow(0 0 2px rgba(255, 255, 255, 0.2));
  }
}

@media (max-width: 768px) {
  .splash-head {
    font-size: 1.8rem;
  }
}

/* trash */
.trash-can {
  position: absolute;
  bottom: 30px;
  right: 30px;
  font-size: 80px;
  cursor: grab;
  z-index: 15;
}

.trash-can:active {
  cursor: grabbing;
}

.baseball.collected {
  animation: none;
  pointer-events: none;
}

.btn {
  display: inline-block;
  padding: 12px 30px;
  font-size: 1rem;
  font-weight: 600;
  text-decoration: none;
  border-radius: 8px;
  margin: 5px;
  cursor: pointer;
  position: relative;
  z-index: 10;
}

.btn-consent {
  background: white;
  color: #1f8dd6;
  animation: breathe 2s ease-in-out infinite;
  box-shadow: 0 0 12px rgba(59, 130, 246, 0.6);
}

@keyframes breathe {
  0% {
    transform: scale(1);
    box-shadow: 0 0 12px rgba(255, 255, 255, 0.6);
  }
  50% {
    transform: scale(1.08);
    box-shadow: 0 0 20px rgba(255, 255, 255, 0.9);
  }
  100% {
    transform: scale(1);
    box-shadow: 0 0 12px rgba(255, 255, 255, 0.6);
  }
}

.btn-decline {
  background: white;
  color: #1f8dd6;
}

@media (max-width: 768px) {
  .splash-head {
    font-size: 1.8rem;
  }
}

/* trash */
.trash-can {
  position: absolute;
  bottom: 30px;
  right: 30px;
  font-size: 80px;
  cursor: grab;
  z-index: 15;
}

.trash-can:active {
  cursor: grabbing;
}

.baseball.collected {
  animation: none;
  pointer-events: none;
}
</style>
