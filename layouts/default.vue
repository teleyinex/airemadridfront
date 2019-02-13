<template>
  <v-app dark>
    <v-content>
      <v-container>
        <nuxt />
      </v-container>
    </v-content>
    <v-footer style="font-size: 12px;">
      <div style="padding-left: 5px;">&copy; <a style="color: white;"
           href="https://daniellombrana.es">Daniel Lombraña</a></div>
      <v-spacer></v-spacer>
      <div style="padding-right: 5px;">Datos del <a style="color: white;"
           href="http://www.mambiente.munimadrid.es/sica/scripts/index.php?menu=consulta&smenu=reports&link=data">Ayto.
           Madrid</a></div>
    </v-footer>
  </v-app>
</template>

<script>
export default {
  data() {
    return {
      clipped: false,
      drawer: false,
      fixed: false,
      items: [
        {
          icon: 'apps',
          title: 'Welcome',
          to: '/'
        },
        {
          icon: 'bubble_chart',
          title: 'Inspire',
          to: '/inspire'
        }
      ],
      miniVariant: false,
      right: true,
      rightDrawer: false,
      title: 'Vuetify.js'
    }
  }
}
</script>
