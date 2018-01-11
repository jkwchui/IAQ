<template lang="pug">
q-layout(ref="layout" view="lHh Lpr fff" :left-class="{'bg-grey-2': true}")
  q-toolbar(slot="header" color="secondary")
    q-btn(flat="" @click="$refs.layout.toggleLeft()")
      q-icon(name="menu")
    q-toolbar-title
      | IA/4 Request Management
      //- div(slot="subtitle") Running on Quasar v{{$q.version}}
  div(slot="left")
    //
      Use <q-side-link> component
      instead of <q-item> for
      internal vue-router navigation
    q-list(no-border="" link="" inset-delimiter="")
      q-list-header
        .text-bold.big REQUESTS
        q-side-link(to="/requests" dense)
          q-item-main
            q-item-tile(label)
              p Requests
        q-side-link(to="/reagents" dense)
          q-item-main
            q-item-tile(label)
              p By reagents
        //- q-side-link()
      //- q-item(@click="launch('http://quasar-framework.org')")
      //-   q-item-side(icon="school")
      //-     q-item-main(label="Docs" sublabel="quasar-framework.org")
      
      q-list-header Inventory
      //- q-item(@click="launch('https://twitter.com/quasarframework')")
      //-   q-item-side(icon="rss feed")
      //-     q-item-main(label="Twitter" sublabel="@quasarframework")
  // Replace following <div> with <router-view /> component if using subRoutes
  
  //- .layout-padding
  router-view
  //-   .logo(:style="position")
  //-     img(src="~assets/quasar-logo-full.svg")

</template>

<script>
import {
  QLayout,
  QToolbar,
  QToolbarTitle,
  QBtn,
  QIcon,
  QList,
  QListHeader,
  QItem,
  QItemSide,
  QItemMain,
  QItemTile,
  QSideLink
} from 'quasar'

export default {
  name: 'index',
  components: {
    QLayout,
    QToolbar,
    QToolbarTitle,
    QBtn,
    QIcon,
    QList,
    QListHeader,
    QItem,
    QItemSide,
    QItemMain,
    QItemTile,
    QSideLink
  },
  data () {
    return {
    }
  },
  computed: {
  },
  methods: {
  },
  mounted () {
  }
}
</script>

<style lang="stylus">
</style>
