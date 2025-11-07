<template>
  <div>
    <div class="header">
      <div class="home-menu pure-menu pure-menu-horizontal pure-menu-fixed">
        <a class="pure-menu-heading" href="">Welcome</a>
        <ul class="pure-menu-list">
          <li class="pure-menu-item pure-menu-selected">
            <a class="pure-menu-link" href="">Home</a>
          </li>
        </ul>
      </div>
    </div>

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
        <h2 class="content-head is-center">Should the Minnesota Twins keep Pablo Lopez?</h2>

        <div class="pure-g">
          <div class="l-box-lrg pure-u-1 pure-u-md-2-5">
            <form class="pure-form pure-form-stacked" id="survey">
              <fieldset>
                <legend>Describe your feelings on Pablo Lopez's tenure so far with the team</legend>
                <input
                  id="q1"
                  type="text"
                  name="q1"
                  placeholder="Fill in the blank"
                  required
                  minlength="3"
                />

                <legend>How do you view Pablo Lopez as?</legend>
                <div class="radio-group">
                  <label class="pure-radio">
                    Washed
                    <input type="radio" name="q2" value="washed" />
                  </label>
                  <label class="pure-radio">
                    Staff Ace
                    <input type="radio" name="q2" value="staff_ace" />
                  </label>
                  <label class="pure-radio">
                    Cy Young Award
                    <input type="radio" name="q2" value="cy_young" />
                  </label>
                </div>

                <legend>How do you view Pablo Lopez's current contract?</legend>
                <select id="q3" name="q3">
                  <option>Overpaid</option>
                  <option>Fair</option>
                  <option>Great Value</option>
                </select>

                <legend>Should the Minnesota Twins keep Pablo Lopez?</legend>
                <div class="checkbox-group">
                  <label class="pure-checkbox">
                    Yes
                    <input id="q4" type="checkbox" name="q4" value="yes" v-model="q4" />
                  </label>
                </div>

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
              </fieldset>

              <button type="submit" class="pure-button pure-button-primary">Next</button>
            </form>
          </div>
        </div>
      </div>
    </motion.div>
  </div>
</template>

<script setup>
import { motion } from 'motion-v'
import { ref, onMounted } from 'vue'

defineOptions({ name: 'SurveyForm' })

const scrollY = ref(0)
onMounted(() => {
  window.addEventListener('scroll', () => {
    scrollY.value = window.scrollY
  })
})

const conditionsText = ref('')
const q4 = ref(false)
</script>

<style src="../assets/SurveyForm.css"></style>
