<template>

  <v-card
    class="mx-auto"
    outlined
  >
    <v-card-text class="font--text">
      <div class="subtitle-1 mb-1 font-weight-bold">Global Controls</div>
      <div class="caption mb-1 font-weight-bold">What unit do you use?</div>

      <v-row>
        <v-col
          cols="12"
          sm="12"
          md="12"
        >
          <v-radio-group
            v-model="temperature_unit"
            :row="radio_group_display.row"
            :column="radio_group_display.column"
          >
            <template v-slot:label>
              <div><strong>Temperature Unit:</strong></div>
            </template>

            <v-radio
              value="F"
              color="secondary"
            >
              <template v-slot:label>
                <div>Fahrenheit</div>
              </template>
            </v-radio>
            <v-radio
              value="C"
              color="secondary"
            >
              <template v-slot:label>
                <div>Celcius</div>
              </template>
            </v-radio>
          </v-radio-group>
        </v-col>
      </v-row>

      <v-row>
        <v-col
          cols="12"
          sm="12"
          md="12"
        >
          <v-radio-group
            v-model="volume_unit"
            :row="radio_group_display.row"
            :column="radio_group_display.column"
          >
            <template v-slot:label>
              <div><strong>Volume Unit:</strong></div>
            </template>

            <v-radio
              value="US"
              color="secondary"
            >
              <template v-slot:label>
                <div>Imperial (eg. cups)</div>
              </template>
            </v-radio>
            <v-radio
              value="SI"
              color="secondary"
            >
              <template v-slot:label>
                <div>Metric (eg. ml)</div>
              </template>
            </v-radio>
          </v-radio-group>
        </v-col>
      </v-row>

    </v-card-text>
  </v-card>

</template>

<script>
  export default {
    name: 'Controls',

    props: {
      temperatureUnit: String,
      volumeUnit: String
    },

    computed: {
      radio_group_display: function() {
        let vm = this;
        const { xs, sm } = vm.$vuetify.breakpoint;

        if (xs || sm) {
          return {
            row: false,
            column: true
          };
        } else {
          return {
            row: true,
            column: false
          };
        }
      },
    },

    mounted() {
      this.init();
    },

    data: () => ({
      temperature_unit: null,
      volume_unit: null,
    }),

    methods: {
      init: function() {
        let vm = this;
        vm.temperature_unit = vm.temperatureUnit;
        vm.volume_unit = vm.volumeUnit;
      },
    },

    watch: {
      temperature_unit: function(new_val) {
        this.$emit('update:temperatureUnit', new_val);
      },

      volume_unit: function(new_val) {
        this.$emit('update:volumeUnit', new_val);
      }
    },
  }
</script>
