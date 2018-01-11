<template lang="pug">
  .student-request
    q-card
      q-card-title(:class="{ 'bg-blue-2': req.subject === 'Chemistry', 'bg-grey-3': req.subject === 'Physics', 'bg-green-2': req.subject === 'Biology', 'bg-amber-2': req.subject === 'ESS'}")
        .row
          .col-9
            div(v-if="req.subject === 'Physics'" :value="req.class") 
              q-chip(color="blue-grey") {{ req.uid }}
            div(v-if="req.subject === 'Chemistry'" :value="req.class") 
              q-chip(color="primary") {{ req.uid }}
            div(v-if="req.subject === 'Biology'" :value="req.class") 
              q-chip(color="secondary") {{ req.uid }}
            div(v-if="req.subject === 'ESS'" :value="req.class") 
              q-chip(color="warning") {{ req.uid }}
          .col-3
            p.text-italic.small Submitted: {{ req.timestamp | moment("calendar") }}

        .row
          .col-9
            h3 #[b {{ req.name }}]
          .col-3
            p {{ req.subject }} ({{ req.teacher }})
            //- el-badge(v-if="req.subject === 'Physics'" :value="req.class")

      q-card-separator

      q-card-main
        .row
          p.text-bold {{ req.RQ }}
        
        .row.text-grey-7
          .col-6 
            p #[i Independent Variable]: {{ req.indep }}
          .col-6
            p #[i Dependent Variable]: {{ req.dep }}

      q-card-separator

      q-card-main
        div(v-if="req.digital_sensors.length !== 0") 
          h4 Sensors
          q-list 
            q-item(v-for="sensor in req.digital_sensors" :key="sensor") {{ sensor }}
          br
          p(v-if="req.more_equipment !== ''") {{ req.more_equipment }}

        div(v-if="reagents.length !== 0") 
          h4 Reagents
          .container
            q-data-table(:data="reagents", :config="reagentTableConfig", :columns="reagentTableColumns")
            br
            p(v-if="req.more_reagents !== ''") {{ req.more_reagents }}

        div(v-if="purchases.length !== 0") 
          h4 Purchases
          q-data-table(:data="purchases", :config="purchasesTableConfig", :columns="purchasesTableColumns")
          br
          p(v-if="req.more_purchases !== ''") {{ req.more_purchases }}
        
        div(v-if="req.custom.item !== ''") 
          h4 Custom Material
          p
            a(:href="req.custom.drawing") {{ req.custom.item }}
          p link to drawing: {{ req.custom.drawing }}

        div(v-if="req.method !== ''") 
          h4 
            a(:href="req.method") Method
            p link to method: {{ req.method }}
        //- button(type="button" @click="print") Print

</template>

<script>
// import moment from 'moment'
// import printJS from 'print-js'

import {
  QCard,
  QCardTitle,
  QCardMain,
  QCardMedia,
  QCardSeparator,
  QChip,
  QList,
  QItem,
  QDataTable
} from 'quasar'

export default {
  name: 'request',
  data () {
    return {
      reagents: [],
      purchases: [],
      reagentTableConfig: {
        rowHeight: '50px',
        // title: '',
        selection: 'multiple'
      },
      reagentTableColumns: [
        {
          label: 'Name',
          field: 'item',
          width: '250px'
          // filter: true,
          // sort:
        },
        {
          label: 'Amount / g',
          field: 'amount',
          width: '100px'
          // filter: true,
          // sort:
        },
        {
          label: 'Conc',
          field: 'conc',
          width: '100px'
          // filter: true,
          // sort:
        },
        {
          label: 'Volume',
          field: 'vol',
          width: '100px'
          // filter: true,
          // sort:
        }
      ],
      purchasesTableConfig: {
        rowHeight: '50px',
        // title: '',
        selection: 'multiple'
      },
      purchasesTableColumns: [
        {
          label: 'Name',
          field: 'item',
          width: '250px'
          // filter: true,
          // sort:
        },
        {
          label: 'Amount',
          field: 'amount',
          width: '100px'
          // filter: true,
          // sort:
        },
        {
          label: 'Approx. cost',
          field: 'cost',
          width: '100px'
          // filter: true,
          // sort:
        },
        {
          label: 'Link to supplier',
          field: 'link',
          width: '250px'
          // filter: true,
          // sort:
        }
      ]
    }
  },

  components: {
    QCard,
    QCardTitle,
    QCardMain,
    QCardMedia,
    QCardSeparator,
    QChip,
    QList,
    QItem,
    QDataTable
  },

  props: ['req'],

  computed: {
  },

  watch: {
  },

  created () {
    // console.log(printJS)
    this.loadReagents()
    this.loadPurchases()
  },

  methods: {
    loadReagents () {
      for (let r of this.req.reagents) {
        let formattedReagent = {
          item: r.item,
          amount: r.amount,
          conc: r.conc,
          vol: r.vol
        }

        if (formattedReagent.item !== '') {
          this.reagents.push(formattedReagent)
        }
      }

      this.reagents.sort((a, b) => {
        if (a.item.toLowerCase() < b.item.toLowerCase()) {
          return -1
        }
        else if (a.item.toLowerCase() > b.item.toLowerCase()) {
          return 1
        }
        return 0
      })
    },

    loadPurchases () {
      for (let p of this.req.requests) {
        let formattedRequest = {
          item: p.item,
          amount: p.amount,
          link: p.link,
          cost: p.cost
        }

        if (formattedRequest.item !== '') {
          this.purchases.push(formattedRequest)
        }
      }

      this.purchases.sort((a, b) => {
        if (a.item.toLowerCase() < b.item.toLowerCase()) { return -1 }
        else if (a.item.toLowerCase() > b.item.toLowerCase()) { return 1 }
        else { return 0 }
      })
    }
  }
}
</script>

<style>
  .student-request {
    page-break-after: always;
  }
  .card-info {
    background-color: pink-2;
  }
</style>