<template lang="pug">
  .layout-padding
    h1 PURCHASES
    br
    q-data-table(:data="purchases", :config="config", :columns="columns")
      template(slot="col-link" scope="cell")
        a(:href="cell.data") {{ cell.data }}
      template(slot="col-email" scope="cell")
        a(:href="'mailto:'+cell.data+'?Subject=[Lab%20Request]%20'") {{ cell.data }}
    //- hr
    h2 Additional purchases
    q-data-table(:data="additionalPurchases", :config="config", :columns="addColumns")

</template>

<script>
import {
  QDataTable,
  QList,
  QItem
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
          label: 'Purchase',
          field: 'item',
          width: '180px'
        },
        {
          label: 'Amount',
          field: 'amount',
          width: '80px'
        },
        {
          label: 'Supplier Link',
          field: 'link',
          width: '250px'
        },
        {
          label: 'Approx cost',
          field: 'cost',
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
        },
        {
          label: 'email',
          field: 'email',
          width: '100px'
        }
      ],
      addColumns: [
        {
          label: 'Item',
          field: 'item',
          width: '180px'
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
        },
        {
          label: 'email',
          field: 'email',
          width: '100px'
        }
      ]
    }
  },

  components: {
    QDataTable,
    QList,
    QItem
  },

  computed: {
    requests () {
      return this.$store.getters.getRequests
    },

    purchases () {
      let tmpPurchases = []

      for (let request of this.requests) {
        // console.log(request)
        for (let purchase of request.requests) {
          // console.log(reagent)
          let formattedPurchase = {
            item: purchase.item,
            amount: purchase.amount,
            link: purchase.link,
            cost: purchase.cost,
            name: request.name,
            subject: request.subject,
            class: request.class,
            uid: request.uid,
            email: request.email
          }

          // console.log("formattedReagent= ", formattedReagent)
          if (formattedPurchase.item !== '') {
            tmpPurchases.push(formattedPurchase)
          }
        }
      }
      tmpPurchases.sort((a, b) => {
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
      return tmpPurchases
    },

    additionalPurchases () {
      let tmpAdditionalPurchases = []
      for (let request of this.requests) {
        let formattedAdditionalPurchases = {
          item: request.more_purchases,
          name: request.name,
          subject: request.subject,
          class: request.class,
          uid: request.uid,
          email: request.email
        }
        if (formattedAdditionalPurchases.item !== '') {
          tmpAdditionalPurchases.push(formattedAdditionalPurchases)
        }
      }
      return tmpAdditionalPurchases
    }
  }
}
</script>

<style>
</style>
