<template>
  <div>
    <motion.div
      class="splash-container"
      :initial="{ opacity: 1, y: 0 }"
      :animate="{ opacity: scrollY > 100 ? 0 : 1, y: scrollY > 100 ? -50 : 0 }"
      :transition="{ duration: 0.6 }"
    >
      <div class="splash">
        <h1 class="splash-head">Complete the survey below</h1>
      </div>
    </motion.div>

    <motion.div
      class="content-wrapper"
      :initial="{ y: 100, opacity: 0 }"
      :whileInView="{ y: 0, opacity: 1 }"
      :transition="{ duration: 0.8, ease: 'easeOut' }"
    >
      <div class="content">
        <h2 class="content-head is-center">
          <span>{{ typed }}</span
          ><span class="cursor">|</span>
        </h2>
        <div class="pure-g">
          <div class="l-box-lrg pure-u-1 pure-u-md-2-5">
            <form class="pure-form pure-form-stacked" id="survey">
              <fieldset>
                <legend>Describe your feelings on Pablo Lopez's tenure so far with the team</legend>
                <motion.input
                  id="q1"
                  type="text"
                  name="q1"
                  placeholder="Fill in the blank"
                  :whileFocus="{ scale: 1.05, borderColor: 'rgb(31, 141, 214)' }"
                  :transition="{ type: 'spring', stiffness: 300 }"
                  required
                  minlength="3"
                />

                <legend>How do you view Pablo Lopez as?</legend>
                <div class="radio-group">
                  <motion.label
                    v-for="(option, index) in radioOptions"
                    :key="option.value"
                    class="pure-radio"
                    :initial="{ opacity: 0, x: -20 }"
                    :whileInView="{ opacity: 1, x: 0 }"
                    :transition="{ delay: index * 0.1, duration: 0.4 }"
                    :whileHover="{ scale: 1.05, x: 5 }"
                  >
                    {{ option }}
                    <input type="radio" name="q2" :value="option" />
                  </motion.label>
                </div>

                <legend>How do you view Pablo Lopez's current contract?</legend>
                <motion.select
                  id="q3"
                  name="q3"
                  :whileHover="{ scale: 1.1, rotate: 1 }"
                  :transition="{ type: 'spring', stiffness: 100, damping: 5 }"
                >
                  <option>Overpaid</option>
                  <option>Fair</option>
                  <option>Great Value</option>
                </motion.select>

                <legend>Should the Minnesota Twins keep Pablo Lopez?</legend>
                <div class="checkbox-group">
                  <label class="pure-checkbox">
                    Yes
                    <button
                      type="button"
                      :class="['toggle-container', q4 ? 'end' : 'start']"
                      @click="toggleSwitch"
                    >
                      <motion.div
                        :data-state="q4"
                        class="toggle-handle"
                        layout
                        :transition="{
                          type: 'spring',
                          visualDuration: 0.2,
                          bounce: 0.2,
                        }"
                      />
                    </button>
                  </label>
                </div>

                <AnimatePresence>
                  <motion.div
                    v-if="q4"
                    id="q4_text"
                    style="margin-top: 1em"
                    :initial="{ opacity: 0, y: -20 }"
                    :animate="{ opacity: 1, y: 0 }"
                    :exit="{ opacity: 0, y: -20 }"
                    :transition="{ duration: 0.5, ease: 'easeOut' }"
                  >
                    <label for="q4_text_input">Please describe why:</label>
                    <motion.textarea
                      id="q4_text_input"
                      name="q4_text"
                      style="width: 100%"
                      rows="4"
                      v-model="conditionsText"
                      :initial="{ opacity: 0 }"
                      :animate="{ opacity: 1 }"
                      :transition="{ duration: 0.6, delay: 0.2 }"
                    />
                  </motion.div>
                </AnimatePresence>
              </fieldset>

              <motion.button
                type="submit"
                class="pure-button pure-button-primary"
                :whileHover="{ scale: 1.15, rotate: 3 }"
                :whileTap="{ scale: 0.9, rotate: -5 }"
                :transition="{ type: 'spring', stiffness: 400, damping: 17 }"
              >
                Next
              </motion.button>
            </form>
            <div style="display: flex; justify-content: center;">
              <motion.img
                src="/pb-throw.jpg"
                alt="Pablo Lopez throwing a pitch"
                style="width: 400px; height: auto"
                class="pablo-image"
                :initial="{ opacity: 0, y: 50 }"
                :animate="{ opacity: 1, y: scrollY * -0.2, rotate: scrollY * 0.02 }"
                :transition="{ type: 'spring', stiffness: 120, damping: 20 }"
              />
            </div>
          </div>
        </div>
      </div>
    </motion.div>

    <motion.div
      id="scroll-indicator"
      class="scroll-indicator"
      :initial="{ scaleX: 0 }"
      :animate="{ scaleX: indicatorScale }"
      :transition="{ type: 'spring', stiffness: 120, damping: 14 }"
    />
  </div>
</template>

<script setup>
import { motion, AnimatePresence, useScroll, animate } from 'motion-v'
import { ref, onMounted, onBeforeUnmount } from 'vue'

const radioOptions = ['Washed', 'Staff Ace', 'Cy Young Award']

defineOptions({ name: 'SurveyForm' })

const { scrollYProgress } = useScroll()

const scrollY = ref(0)
const onScroll = () => (scrollY.value = window.scrollY)

const indicatorScale = ref(0)

onMounted(() => {
  scrollYProgress.onChange((v) => {
    indicatorScale.value = v
  })
  indicatorScale.value = scrollYProgress.get()

  window.addEventListener('scroll', onScroll)
  onScroll()

  const text = question.value
  controls = animate(0, text.length, {
    duration: text.length * 0.1,
    ease: 'linear',
    onUpdate: (v) => {
      typed.value = text.slice(0, Math.floor(v))
    },
  })
})

onBeforeUnmount(() => window.removeEventListener('scroll', onScroll))

const conditionsText = ref('')

const q4 = ref(false)

const toggleSwitch = () => {
  q4.value = !q4.value
}

const question = ref('Should the Minnesota Twins keep Pablo Lopez?')
const typed = ref('')
let controls
</script>

<style src="../assets/survey.css"></style>

<style scoped>
.splash {
  margin-top: 0;
}
.scroll-indicator {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 7px;
  background-color: rgb(31, 141, 214);
  transform-origin: left;
  z-index: 9999;
}

.toggle-container {
  width: 60px;
  height: 30px;
  background-color: var(--hue-3-transparent);
  border-radius: 50px;
  cursor: pointer;
  display: flex;
  align-items: center;
  padding: 10px;
  overflow: hidden;
  box-sizing: border-box;
}

.toggle-container.start {
  justify-content: flex-start;
}

.toggle-container.end {
  justify-content: flex-end;
}

.toggle-handle {
  width: calc(40px - 2 * 10px);
  height: calc(40px - 2 * 10px);
  background-color: #9911ff;
  border-radius: 50%;
}

.cursor {
  display: inline-block;
  animation: blink 1s steps(1) infinite;
}

.pablo-image {
  margin-top: 200px;
  border-radius: 12px;
  box-shadow: 0 6px 14px rgba(20, 20, 40, 0.1);
}

@keyframes blink {
  50% {
    opacity: 0;
  }
}
</style>
