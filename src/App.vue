<template lang="pug">
  .q-app
    router-view
</template>

<script>

import sheetsy from 'sheetsy'
import moment from 'moment'

import StudentRequest from './components/StudentRequest.vue'

/*
 * Root component
 */
export default {
  data () {
    return {
      workbook: {},
      formdata: {},
      selectedId: 2,
      // requests: []
      requests () {
        return this.$store.getters.getRequest
      }
    }
  },

  computed: {
  },

  components: {
    StudentRequest
    // ReqNav
  },

  mounted () {
    // setup Sheetsy to read a Google Sheet
    const url = 'https://docs.google.com/spreadsheets/d/1fYnm8Z7ixspP2nzOm3REz2Co9QRcJ5wDD1eNsIKMkZw/'
    const { urlToKey, getWorkbook, getSheet } = sheetsy
    const key = urlToKey(url)

    console.log(urlToKey(url))

    getWorkbook(key).then(workbook => {
      this.workbook = workbook
    })

    getSheet(key, 'owrromh').then(sheet => {
      this.formdata = sheet
      this.loadCohort(this.formdata)
    })
  },

  beforeDestroy () {

  },

  methods: {
    loadRequest (row, sheet) {
      let r = sheet.rows[row]

      let request = {
        uid: r.id,
        name: r.name,
        subject: r.subject,
        class: r.classblock,
        teacher: r.teacher,
        timestamp: moment(r.timestamp, 'DD-MM-YYYY hh:mm:ss'),

        RQ: r.researchquestion,
        indep: r.independentvariable,
        dep: r.dependentvariable,

        digital_sensors: r.digitalsensors.split(', '),
        more_equipment: r.additionalequipmentrequestsnopurchasingrequired,

        reagents: [
          {
            item: r.reagent1,
            amount: r.reagent1amountgrequiredifsolid,
            conc: r.reagent1concentrationsrequiredmoldm3ifsolution,
            vol: r.reagent1volumerequiredcm3ifsolution
          },
          {
            item: r.reagent2,
            amount: r.reagent2amountgrequiredifsolid,
            conc: r.reagent2concentrationsrequiredmoldm3ifsolution,
            vol: r.reagent2volumerequiredcm3ifsolution
          },
          {
            item: r.reagent3,
            amount: r.reagent3amountgrequiredifsolid,
            conc: r.reagent3concentrationsrequiredmoldm3ifsolution,
            vol: r.reagent3volumerequiredcm3ifsolution
          },
          {
            item: r.reagent4,
            amount: r.reagent4amountgrequiredifsolid,
            conc: r.reagent4concentrationsrequiredmoldm3ifsolution,
            vol: r.reagent4volumerequiredcm3ifsolution
          },
          {
            item: r.reagent5,
            amount: r.reagent5amountgrequiredifsolid,
            conc: r.reagent5concentrationsrequiredmoldm3ifsolution,
            vol: r.reagent5volumerequiredcm3ifsolution
          },
          {
            item: r.reagent6,
            amount: r.reagent6amountgrequiredifsolid,
            conc: r.reagent6concentrationsrequiredmoldm3ifsolution,
            vol: r.reagent6volumerequiredcm3ifsolution
          }
        ],
        more_reagents: r.additionalchemicalrequestsnotrequiringpurchase,

        requests: [
          {
            item: r.request1nameofitem,
            amount: r.request1amountrequired,
            link: r.request1linktoacommercialsupplier,
            cost: r.request1approximatecost
          },
          {
            item: r.request2nameofitem,
            amount: r.request2amountrequired,
            link: r.request2linktoacommercialsupplier,
            cost: r.request2approximatecost
          },
          {
            item: r.request3nameofitem,
            amount: r.request3amountrequired,
            link: r.request3linktoacommercialsupplier,
            cost: r.request3approximatecost
          }
        ],
        more_purchases: r.additionalpurchasingrequests,

        custom: {
          item: r.custommaterialrequest,
          drawing: r.supplementarydrawing
        },

        method: r.methodupload
      }

      return request
    },

    loadCohort (sheet) {
      let reqs = []
      for (let row = 1; row < this.formdata.rows.length; row++) {
        reqs.push(this.loadRequest(row, sheet))
      }

      this.$store.commit('setRequests', reqs)
    }
  }
}
</script>

<style></style>
