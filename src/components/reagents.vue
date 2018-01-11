<template lang="pug">
  .layout-padding
    h1 REAGENTS
    br
    q-data-table(:data="reagents", :config="config", :columns="columns")
</template>

<script>
import {
  QDataTable
} from 'quasar'

export default {
  data () {
    return {
      config: {
        rowHeight: '50px',
        columnPicker: true,
        leftStickyColumns: 1,
        selection: 'multiple'
      },
      columns: [
        {
          label: 'Reagent',
          field: 'item',
          width: '200px'
        },
        {
          label: 'Amount / g',
          field: 'amount',
          width: '80px'
        },
        {
          label: 'Conc / mol/dm3',
          field: 'conc',
          width: '100px'
        },
        {
          label: 'Volume / cm3',
          field: 'vol',
          width: '100px'
        },
        {
          label: 'Student',
          field: 'name',
          width: '100px'
        },
        {
          label: 'Subject',
          field: 'subject',
          width: '80px'
        },
        {
          label: 'Block',
          field: 'class',
          width: '60px'
        },
        {
          label: 'ID',
          field: 'uid',
          width: '130px'
        }
      ]
    }
  },

  components: {
    QDataTable
  },

  computed: {
    requests () {
      return this.$store.getters.getRequests
    },

    reagents () {
      let tmpReagents = []

      for (let request of this.requests) {
        // console.log(request)
        for (let reagent of request.reagents) {
          // console.log(reagent)
          let formattedReagent = {
            item: reagent.item,
            amount: reagent.amount,
            conc: reagent.conc,
            vol: reagent.vol,
            name: request.name,
            subject: request.subject,
            class: request.class,
            uid: request.uid
          }

          // console.log("formattedReagent= ", formattedReagent)
          if (formattedReagent.item !== '') {
            tmpReagents.push(formattedReagent)
          }
        }
      }
      tmpReagents.sort((a, b) => {
        if (a.class.toLowerCase() < b.class.toLowerCase()) { return -1 }
        else if (a.class.toLowerCase() > b.class.toLowerCase()) { return 1 }
        else { return 0 }
      }).sort((a, b) => {
        if (a.subject.toLowerCase() < b.subject.toLowerCase()) { return -1 }
        else if (a.subject.toLowerCase() > b.subject.toLowerCase()) { return 1 }
        else { return 0 }
      }).sort((a, b) => {
        if (a.item.toLowerCase() < b.item.toLowerCase()) { return -1 }
        else if (a.item.toLowerCase() > b.item.toLowerCase()) { return 1 }
        else { return 0 }
      })
      return tmpReagents
    }
  }
}
</script>

<style>
</style>
