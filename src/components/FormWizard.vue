<template>
  <div>
    <div v-if="wizardInProgress" v-show="asyncState !== 'pending'">
      <keep-alive>
        <component
          ref="currentStep"
          :is="currentStep"
          @updateAsyncState="updateAsyncState"
          :wizard-data="form"
        ></component>
      </keep-alive>

      <div class="progress-bar">
        <div :style="`width: ${progress}%;`"></div>
      </div>

      <!-- Actions -->
      <div class="buttons">
        <button
          @click="goBack"
          v-if="currentStepNumber > 1"
          class="btn-outlined"
        >Back
        </button>

        <button
          @click="nextButtonAction"
          class="btn"
        >{{isLastStep ? 'Complete Order' : 'Next'}}</button>
      </div>
    </div>

    <div v-else>
      <h1 class="title">Thank you! {{form.name}} with {{form.tag[0].text}} Tag</h1>
    </div>

    <div class="loading-wrapper" v-if="asyncState === 'pending'">
      <div class="loader">
        <img src="/spinner.svg" alt="">
        <p>Please wait, we're hitting our servers!</p>
      </div>
    </div>
  </div>
</template>

<script>
import {postFormToDB} from '../api'
import FormUserDetails from './FormUserDetails'
import FormTags from './FormTags'
export default {
  name: 'FormWizard',
  components: {
    FormUserDetails,
    FormTags
  },
  data () {
    return {
      currentStepNumber: 1,
      asyncState: null,
      steps: [
        'FormUserDetails',
        'FormTags'
      ],
      form: {
        email: null,
        name: null,
        password: null,
        tag: null
      }
    }
  },
  computed: {
    isLastStep () {
      return this.currentStepNumber === this.length
    },
    wizardInProgress () {
      return this.currentStepNumber <= this.length
    },
    length () {
      return this.steps.length
    },
    currentStep () {
      return this.steps[this.currentStepNumber - 1]
    },
    progress () {
      return this.currentStepNumber/this.length * 100
    }
  },
  methods: {
    updateAsyncState (state) {
      this.asyncState = state
    },
    submitOrder () {
      this.asyncState = 'pending'
      postFormToDB(this.form)
        .then(() => {
          console.log('form submitted', this.form)
          this.asyncState = 'success'
          this.currentStepNumber++
        })
    },
    nextButtonAction () {
      this.$refs.currentStep.submit()
        .then(stepData => {
          Object.assign(this.form, stepData)
          if (this.isLastStep) {
            this.submitOrder()
          } else {
            this.goNext()
          }
        })
        .catch(error => console.log(error))
    },
    goBack () {
      this.currentStepNumber--
    },
    goNext () {
      this.currentStepNumber++
    }
  }
}
</script>
