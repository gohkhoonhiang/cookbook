<template>

  <v-card flat>
    <v-tabs
      show-arrows
    >
      <v-tab key="baking_recipes_tab">
        <v-icon left small>mdi-bread-slice-outline</v-icon>
        Baking
      </v-tab>

      <v-tab-item key="baking_recipes_tab">

        <v-card flat>
        </v-card>
      </v-tab-item>
    </v-tabs>

  </v-card>

</template>

<script>
  export default {
    name: 'Recipes',

    props: {
      temperature_unit: String,
      volume_unit: String,

    },

    data: () => ({
    }),

    methods: {
    },

    watch: {
    },
  }
</script>
