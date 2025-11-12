<script setup>
import { motion, useAnimate } from 'motion-v'
import { onMounted, ref } from 'vue'

const [scope, animate] = useAnimate()

// Create baseball rain
const baseballs = ref([])
const createBaseballs = () => {
  for (let i = 0; i < 20; i++) {
    baseballs.value.push({
      id: i,
      left: (i * 5) + (Math.random() * 3),
      delay: Math.random() * 5,
      duration: 3 + Math.random() * 2
    })
  }
}

onMounted(() => {
  createBaseballs()

  animate('.splash-head', { opacity: 1, y: 0 }, { duration: 0.8, type: 'spring', stiffness: 100, damping: 10 })
    .then(() => {
      animate('.splash-subhead', { opacity: 1 }, { duration: 0.6 })
      animate('.buttons', { opacity: 1 }, { duration: 0.6, delay: 0.2 })
    })
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
        ease: 'linear'
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
  </div>
</template>

<style scoped>
.splash-container {
  background: #0f172a;
  min-height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 20px;
  position: relative;
  overflow: hidden;
}

.baseball {
  position: absolute;
  font-size: 30px;
  pointer-events: none;
  z-index: 5;
  top: 0;
  filter: opacity(0.8);
}

.splash {
  text-align: center;
  max-width: 600px;
  position: relative;
  z-index: 10;
  background: #0f172a;
  padding: 40px;
  border-radius: 12px;
}

.splash-head {
  font-size: 2.5rem;
  font-weight: 700;
  color: white;
  margin: 0 0 20px;
  position: relative;
  z-index: 10;
  border: 2px solid white;
  border-radius: 10px;
  padding: 10px 15px;
  animation: questionGlow 5s linear infinite;
  box-shadow: 0 0 10px white;           /* soft glow */
}
@keyframes questionGlow {
  0%   { border-color: #3b82f6; box-shadow: 0 0 10px #3b82f6; }  /* blue */
  25%  { border-color: #a855f7; box-shadow: 0 0 10px #a855f7; }  /* purple */
  50%  { border-color: #ec4899; box-shadow: 0 0 10px #ec4899; }  /* pink */
  75%  { border-color: #f59e0b; box-shadow: 0 0 10px #f59e0b; }  /* amber */
  100% { border-color: #3b82f6; box-shadow: 0 0 10px #3b82f6; }  /* back to blue */
}




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


.splash-subhead {
  font-size: 1.2rem;
  color: #94a3b8;
  margin: 0 0 30px;
  position: relative;
  z-index: 10;
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
  background: #3b82f6;
  color: white;
  animation: breathe 2s ease-in-out infinite;
  box-shadow: 0 0 12px rgba(59, 130, 246, 0.6);
}

@keyframes breathe {
  0% {
    transform: scale(1);
    box-shadow: 0 0 12px rgba(59, 130, 246, 0.6);
  }
  50% {
    transform: scale(1.08);
    box-shadow: 0 0 20px rgba(59, 130, 246, 0.9);
  }
  100% {
    transform: scale(1);
    box-shadow: 0 0 12px rgba(59, 130, 246, 0.6);
  }
}

.btn-decline {
  background: #1e293b;
  color: white;
}

@media (max-width: 768px) {
  .splash-head {
    font-size: 1.8rem;
  }
}
</style>
