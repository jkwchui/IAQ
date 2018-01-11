<template lang="pug">
  //- <!-- if you want automatic padding use "layout-padding" class -->
  .layout-padding
    h1 REQUESTS
    br
    q-fixed-position(corner="top-right" :offset="[18, 18]")
      q-checkbox(v-model="showPhys" label="Phys")
      q-checkbox(v-model="showChem" label="Chem")
      q-checkbox(v-model="showBio" label="Bio")
      q-checkbox(v-model="showESS" label="ESS")

    div(v-for="req in requests" :key="req.uid")
      q-transition(appear enter="fadeIn" leave="fadeOut")
        StudentRequest(:req="req" v-if="(req.subject === 'Physics' && showPhys) || (req.subject === 'Chemistry' && showChem) ||(req.subject === 'Biology' && showBio) || (req.subject === 'ESS' && showESS)")
      br
</template>

<script>
import 'quasar-extras/animate/fadeIn.css'
import 'quasar-extras/animate/fadeOut.css'

import StudentRequest from './StudentRequest.vue'
import {
  QCheckbox,
  QFixedPosition,
  QTransition
} from 'quasar'

export default {
  data () {
    return {
      showBio: true,
      showPhys: true,
      showChem: true,
      showESS: true
    }
  },

  props: {
  },

  components: {
    StudentRequest,
    QCheckbox,
    QFixedPosition,
    QTransition
  },

  method: {
    showSubject (r) {
      if (r.subject === 'Physics' && this.showPhys === true) { return true }
    }
  },

  computed: {
    requests () {
      return this.$store.getters.getRequests
    }
  }
}
</script>

<style>
</style>
