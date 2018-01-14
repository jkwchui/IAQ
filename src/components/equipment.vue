<template lang="pug">
  .layout-padding
    h1 SENSORS
    br
    q-data-table(:data="sensors", :config="config", :columns="columns")
      template(slot="col-email" scope="cell")
        a(:href="'mailto:'+cell.data+'?Subject=[Lab%20Request]%20'") {{ cell.data }}
    //- hr
    h2 Additional sensors
    q-data-table(:data="additionalSensors", :config="config", :columns="addColumns")

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
          label: 'Sensor',
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

    sensors () {
      let tmpSensors = []

      for (let request of this.requests) {
        // console.log(request)
        for (let sensor of request.digital_sensors) {
          // console.log(reagent)
          let formattedSensor = {
            item: sensor,
            name: request.name,
            subject: request.subject,
            class: request.class,
            uid: request.uid,
            email: request.email
          }

          // console.log("formattedReagent= ", formattedReagent)
          if (formattedSensor.item !== '') {
            tmpSensors.push(formattedSensor)
          }
        }
      }
      tmpSensors.sort((a, b) => {
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
      return tmpSensors
    },

    additionalSensors () {
      let tmpAdditionalSensors = []
      for (let request of this.requests) {
        let formattedAdditionalSensors = {
          item: request.more_equipment,
          name: request.name,
          subject: request.subject,
          class: request.class,
          uid: request.uid,
          email: request.email
        }
        if (formattedAdditionalSensors.item !== '') {
          tmpAdditionalSensors.push(formattedAdditionalSensors)
        }
      }
      return tmpAdditionalSensors
    }
  }
}
</script>

<style>
</style>
