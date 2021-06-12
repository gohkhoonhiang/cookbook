<template>

  <v-card flat>
    <v-tabs
      show-arrows
    >
      <v-tab key="volume_conversion_tab">
        <v-icon left small>mdi-weight</v-icon>
        Volume
      </v-tab>

      <v-tab key="temperature_conversion_tab">
        <v-icon left small>mdi-thermometer-lines</v-icon>
        Temperature
      </v-tab>

      <v-tab-item key="volume_conversion_tab">

        <v-card flat>

          <v-container fluid>

            <v-row align="center">
              <v-col
                cols="12"
                sm="12"
                md="2"
              >
                <strong>Convert:</strong>
              </v-col>

              <v-col
                cols="12"
                sm="12"
                md="2"
              >
                <v-select
                  v-model="conversion_ingredient"
                  :items="conversion_ingredients"
                  item-text="text"
                  item-value="value"
                  hint="Ingredient"
                  persistent-hint
                  dense
                ></v-select>
              </v-col>

              <v-col
                cols="12"
                sm="12"
                md="2"
              >
                <v-text-field
                  v-model="conversion_from_value"
                  type="text"
                  hint="from how many"
                  clearable
                  persistent-hint
                  dense
                >
                </v-text-field>
              </v-col>

              <v-col
                cols="12"
                sm="12"
                md="2"
              >
                <v-select
                  v-model="conversion_from_unit"
                  :items="conversion_from_units"
                  item-text="text"
                  item-value="value"
                  :hint="conversion_from_unit_hint"
                  persistent-hint
                  dense
                ></v-select>
              </v-col>

              <v-col
                cols="12"
                sm="12"
                md="2"
              >
                <v-text-field
                  v-model="conversion_to_value"
                  type="text"
                  hint="to how many"
                  clearable
                  persistent-hint
                  dense
                >
                </v-text-field>
              </v-col>

              <v-col
                cols="12"
                sm="12"
                md="2"
              >
                <v-select
                  v-model="conversion_to_unit"
                  :items="conversion_to_units"
                  item-text="text"
                  item-value="value"
                  :hint="conversion_to_unit_hint"
                  persistent-hint
                  dense
                ></v-select>
              </v-col>

            </v-row>

            <v-row>
              <v-col
                cols="12"
                sm="12"
                md="12"
              >
                <v-btn
                  color="secondary"
                  block
                  @click="convertIngredient"
                >
                  Convert!
                </v-btn>
              </v-col>
            </v-row>

            <v-row
              v-if="conversion_error"
            >
              <v-col
                cols="12"
                sm="12"
                md="12"
              >
                <p class="red--text text-center text-body-2"><strong>{{ conversion_error }}</strong></p>
              </v-col>
            </v-row>

          </v-container>

        </v-card>
      </v-tab-item>

      <v-tab-item key="temperature_conversion_tab">

        <v-card flat>

          <v-container fluid>

            <v-row align="center">
              <v-col
                cols="12"
                sm="12"
                md="2"
              >
                <strong>Convert:</strong>
              </v-col>

              <v-col
                cols="12"
                sm="12"
                md="2"
              >
                <v-text-field
                  v-model="temperature_from_value"
                  type="number"
                  :hint="temperature_from_unit"
                  clearable
                  persistent-hint
                  dense
                >
                </v-text-field>
              </v-col>

              <v-col
                cols="12"
                sm="12"
                md="2"
              >
                <strong>to/from</strong>
              </v-col>

              <v-col
                cols="12"
                sm="12"
                md="2"
              >
                <v-text-field
                  v-model="temperature_to_value"
                  type="number"
                  :hint="temperature_to_unit"
                  clearable
                  persistent-hint
                  dense
                >
                </v-text-field>
              </v-col>

            </v-row>

            <v-row>
              <v-col
                cols="12"
                sm="12"
                md="12"
              >
                <v-btn
                  color="secondary"
                  block
                  @click="convertTemperature"
                >
                  Convert!
                </v-btn>
              </v-col>
            </v-row>

            <v-row
              v-if="temperature_conversion_error"
            >
              <v-col
                cols="12"
                sm="12"
                md="12"
              >
                <p class="red--text text-center text-body-2"><strong>{{ temperature_conversion_error }}</strong></p>
              </v-col>
            </v-row>

          </v-container>

        </v-card>
      </v-tab-item>

    </v-tabs>
  </v-card>

</template>

<script>
  export default {
    name: 'Calculator',

    props: {
      temperature_unit: String,
      volume_unit: String,

    },

    data: () => ({

      unit_conversion_table: [
        { from_unit: 'cup', to_unit: 'tbsp', conversion_rate: 16, type: 'dry' },
        { from_unit: 'cup', to_unit: 'tsp', conversion_rate: 48, type: 'dry' },
        { from_unit: 'cup', to_unit: 'stick', conversion_rate: 2, type: 'dry' },
        { from_unit: 'tbsp', to_unit: 'tsp', conversion_rate: 3, type: 'dry' },
        { from_unit: 'cup', to_unit: 'ml', conversion_rate: 240, type: 'wet' },
        { from_unit: 'oz', to_unit: 'ml', conversion_rate: 30, type: 'wet' },
        { from_unit: 'tsp', to_unit: 'ml', conversion_rate: 5, type: 'wet' },
        { from_unit: 'tbsp', to_unit: 'ml', conversion_rate: 15, type: 'wet' },
      ],

      ingredient_conversion_table: [
        { ingredient: 'all_purpose_flour', us_amount: 1, us_unit: 'cup', si_amount: 130, si_unit: 'g' },
        { ingredient: 'all_purpose_flour', us_amount: 1, us_unit: 'tbsp', si_amount: 8.125, si_unit: 'g' },
        { ingredient: 'all_purpose_flour', us_amount: 1, us_unit: 'tsp', si_amount: 2.708, si_unit: 'g' },

        { ingredient: 'bread_flour', us_amount: 1, us_unit: 'cup', si_amount: 135, si_unit: 'g' },
        { ingredient: 'bread_flour', us_amount: 1, us_unit: 'tbsp', si_amount: 8.4375, si_unit: 'g' },
        { ingredient: 'bread_flour', us_amount: 1, us_unit: 'tsp', si_amount: 2.8125, si_unit: 'g' },

        { ingredient: 'brown_sugar', us_amount: 1, us_unit: 'cup', si_amount: 220, si_unit: 'g' },
        { ingredient: 'brown_sugar', us_amount: 1, us_unit: 'tbsp', si_amount: 13.75, si_unit: 'g' },
        { ingredient: 'brown_sugar', us_amount: 1, us_unit: 'tsp', si_amount: 4.58, si_unit: 'g' },

        { ingredient: 'butter', us_amount: 1, us_unit: 'tbsp', si_amount: 14.175, si_unit: 'g' },
        { ingredient: 'butter', us_amount: 1, us_unit: 'stick', si_amount: 113, si_unit: 'g' },
        { ingredient: 'butter', us_amount: 1, us_unit: 'cup', si_amount: 226, si_unit: 'g' },

        { ingredient: 'caster_sugar', us_amount: 1, us_unit: 'cup', si_amount: 200, si_unit: 'g' },
        { ingredient: 'caster_sugar', us_amount: 1, us_unit: 'tbsp', si_amount: 12.6, si_unit: 'g' },
        { ingredient: 'caster_sugar', us_amount: 1, us_unit: 'tsp', si_amount: 4.2, si_unit: 'g' },

        { ingredient: 'cornstarch', us_amount: 1, us_unit: 'cup', si_amount: 150, si_unit: 'g' },
        { ingredient: 'cornstarch', us_amount: 1, us_unit: 'tbsp', si_amount: 9.375, si_unit: 'g' },
        { ingredient: 'cornstarch', us_amount: 1, us_unit: 'tsp', si_amount: 3.125, si_unit: 'g' },

        { ingredient: 'cream', us_amount: 1, us_unit: 'cup', si_amount: 240, si_unit: 'g' },
        { ingredient: 'cream', us_amount: 1, us_unit: 'tbsp', si_amount: 15, si_unit: 'g' },
        { ingredient: 'cream', us_amount: 1, us_unit: 'tsp', si_amount: 5, si_unit: 'g' },

        { ingredient: 'flour', us_amount: 1, us_unit: 'cup', si_amount: 113, si_unit: 'g' },
        { ingredient: 'flour', us_amount: 1, us_unit: 'tbsp', si_amount: 7.0625, si_unit: 'g' },
        { ingredient: 'flour', us_amount: 1, us_unit: 'tsp', si_amount: 2.35, si_unit: 'g' },

        { ingredient: 'fresh_milk', us_amount: 1, us_unit: 'cup', si_amount: 240, si_unit: 'ml' },
        { ingredient: 'fresh_milk', us_amount: 1, us_unit: 'tbsp', si_amount: 15, si_unit: 'ml' },
        { ingredient: 'fresh_milk', us_amount: 1, us_unit: 'tsp', si_amount: 5, si_unit: 'ml' },

        { ingredient: 'honey', us_amount: 1, us_unit: 'cup', si_amount: 340, si_unit: 'g' },
        { ingredient: 'honey', us_amount: 1, us_unit: 'tbsp', si_amount: 21.25, si_unit: 'g' },
        { ingredient: 'honey', us_amount: 1, us_unit: 'tsp', si_amount: 7.08, si_unit: 'g' },

        { ingredient: 'icing_sugar', us_amount: 1, us_unit: 'cup', si_amount: 125, si_unit: 'g' },
        { ingredient: 'icing_sugar', us_amount: 1, us_unit: 'tbsp', si_amount: 7.81, si_unit: 'g' },
        { ingredient: 'icing_sugar', us_amount: 1, us_unit: 'tsp', si_amount: 2.60, si_unit: 'g' },

        { ingredient: 'raw_sugar', us_amount: 1, us_unit: 'cup', si_amount: 250, si_unit: 'g' },
        { ingredient: 'raw_sugar', us_amount: 1, us_unit: 'tbsp', si_amount: 15.625, si_unit: 'g' },
        { ingredient: 'raw_sugar', us_amount: 1, us_unit: 'tsp', si_amount: 5.208, si_unit: 'g' },

        { ingredient: 'salt', us_amount: 1, us_unit: 'tbsp', si_amount: 17.07, si_unit: 'g' },
        { ingredient: 'salt', us_amount: 1, us_unit: 'tsp', si_amount: 5.69, si_unit: 'g' },

        { ingredient: 'water', us_amount: 1, us_unit: 'cup', si_amount: 240, si_unit: 'ml' },
        { ingredient: 'water', us_amount: 1, us_unit: 'tbsp', si_amount: 15, si_unit: 'ml' },
        { ingredient: 'water', us_amount: 1, us_unit: 'tsp', si_amount: 5, si_unit: 'ml' },

        { ingredient: 'whole_wheat_flour', us_amount: 1, us_unit: 'cup', si_amount: 128, si_unit: 'g' },
        { ingredient: 'whole_wheat_flour', us_amount: 1, us_unit: 'tbsp', si_amount: 8, si_unit: 'g' },
        { ingredient: 'whole_wheat_flour', us_amount: 1, us_unit: 'tsp', si_amount: 2.667, si_unit: 'g' },

        { ingredient: 'yeast', us_amount: 1, us_unit: 'tbsp', si_amount: 9.3, si_unit: 'g' },
        { ingredient: 'yeast', us_amount: 1, us_unit: 'tsp', si_amount: 3.1, si_unit: 'g' },
      ],

      us_units: [
        { text: 'cup', value: 'cup' },
        { text: 'tbsp', value: 'tbsp' },
        { text: 'tsp', value: 'tsp' },
        { text: 'stick', value: 'stick' },
        { text: 'oz', value: 'oz' },
      ],
      si_units: [
        { text: 'ml', value: 'ml' },
        { text: 'g', value: 'g' },
      ],

      conversion_ingredient: null,
      conversion_ingredients: [
        { text: 'All Purpose Flour', value: 'all_purpose_flour' },
        { text: 'Bread Flour', value: 'bread_flour' },
        { text: 'Brown Sugar', value: 'brown_sugar' },
        { text: 'Butter', value: 'butter' },
        { text: 'Caster Sugar', value: 'caster_sugar' },
        { text: 'Cornstarch', value: 'cornstarch' },
        { text: 'Cream', value: 'cream' },
        { text: 'Flour', value: 'flour' },
        { text: 'Fresh Milk', value: 'fresh_milk' },
        { text: 'Honey', value: 'honey' },
        { text: 'Icing Sugar', value: 'icing_sugar' },
        { text: 'Raw Sugar', value: 'raw_sugar' },
        { text: 'Salt', value: 'salt' },
        { text: 'Water', value: 'water' },
        { text: 'Whole Wheat Flour', value: 'whole_wheat_flour' },
        { text: 'Yeast', value: 'yeast' },
      ],

      conversion_from_value: null,
      conversion_from_unit: null,

      conversion_to_value: null,
      conversion_to_unit: null,

      conversion_error: null,

      temperature_from_value: null,
      temperature_to_value: null,
      temperature_conversion_error: null,
    }),

    computed: {
      conversion_from_units: function() {
        let vm = this;

        if (vm.volume_unit === 'US') {
          if (vm.conversion_ingredient) {
            return [...new Set(vm.ingredient_conversion_table.filter(rate =>
              rate.ingredient === vm.conversion_ingredient
            ).map(r => r.si_unit))];
          } else {
            return vm.si_units;
          }
        } else {
          if (vm.conversion_ingredient) {
            return [...new Set(vm.ingredient_conversion_table.filter(rate =>
              rate.ingredient === vm.conversion_ingredient
            ).map(r => r.us_unit))];
          } else {
            return vm.us_units;
          }
        }
      },

      conversion_to_units: function() {
        let vm = this;

        if (vm.volume_unit === 'US') {
          if (vm.conversion_ingredient && vm.conversion_from_unit) {
            return [...new Set(vm.ingredient_conversion_table.filter(rate =>
              rate.ingredient === vm.conversion_ingredient &&
                rate.si_unit === vm.conversion_from_unit
            ).map(r => r.us_unit))];
          } else {
            return vm.us_units;
          }
        } else {
          if (vm.conversion_ingredient && vm.conversion_from_unit) {
            return [...new Set(vm.ingredient_conversion_table.filter(rate =>
              rate.ingredient === vm.conversion_ingredient &&
                rate.us_unit === vm.conversion_from_unit
            ).map(r => r.si_unit))];
          } else {
            return vm.si_units;
          }
        }
      },

      conversion_from_unit_hint: function() {
        let vm = this;

        if (vm.volume_unit === 'US') {
          return 'Metric unit';
        } else {
          return 'Imperial unit';
        }
      },

      conversion_to_unit_hint: function() {
        let vm = this;

        if (vm.volume_unit === 'US') {
          return 'Imperial unit';
        } else {
          return 'Metric unit';
        }
      },

      temperature_from_unit: function() {
        let vm = this;

        if (vm.temperature_unit === 'F') {
          return 'C';
        } else {
          return 'F';
        }
      },

      temperature_to_unit: function() {
        let vm = this;

        return vm.temperature_unit;
      },

    },

    methods: {
      convertIngredient: function() {
        let vm = this;
        vm.conversion_error = null;

        if (!vm.conversion_from_unit) {
          vm.conversion_error = 'Missing from unit';
          return;
        }

        if (!vm.conversion_to_unit) {
          vm.conversion_error = 'Missing to unit';
          return;
        }

        if (!vm.conversion_from_value && !vm.conversion_to_value) {
          vm.conversion_error = 'One of from or to value must be filled';
          return;
        }

        let ingredient_conversion_rates = vm.ingredient_conversion_table.filter(rate =>
          rate.ingredient === vm.conversion_ingredient
        );

        if (ingredient_conversion_rates.length === 0) {
          vm.conversion_error = 'Conversion rate not available for ingredient';
          return;
        }

        let conversion_rate = null;

        if (vm.volume_unit === 'US') {
          conversion_rate = ingredient_conversion_rates.find(rate =>
            rate.si_unit === vm.conversion_from_unit &&
              rate.us_unit === vm.conversion_to_unit
          );
        } else {
          conversion_rate = ingredient_conversion_rates.find(rate =>
            rate.us_unit === vm.conversion_from_unit &&
              rate.si_unit === vm.conversion_to_unit
          );
        }

        if (!conversion_rate) {
          vm.conversion_error = 'Conversion rate not available for chosen units';
          return;
        }

        if (vm.conversion_from_value) {
          let to_value = null;

          if (vm.volume_unit === 'US') {
            // si -> us
            to_value = conversion_rate.us_amount * vm.conversion_from_value / conversion_rate.si_amount
          } else {
            // us -> si
            to_value = conversion_rate.si_amount * vm.conversion_from_value / conversion_rate.us_amount
          }

          if (isNaN(to_value)) {
            vm.conversion_error = 'Calculation error';
            return;
          }

          vm.conversion_to_value = to_value;
        } else {
          let from_value = null;

          if (vm.volume_unit === 'US') {
            // si <- us
            from_value = conversion_rate.si_amount * vm.conversion_to_value / conversion_rate.us_amount
          } else {
            // us <- si
            vm.conversion_from_value = conversion_rate.us_amount * vm.conversion_to_value / conversion_rate.is_amount
          }

          if (isNaN(from_value)) {
            vm.conversion_error = 'Calculation error';
            return;
          }

          vm.conversion_from_value = from_value;
        }
      },

      convertTemperature: function() {
        let vm = this;

        if (!vm.temperature_from_value && !vm.temperature_to_value) {
          vm.temperature_conversion_error = 'One of from or to value must be filled';
          return;
        }

        if (vm.temperature_unit === 'C') {
          if (vm.temperature_from_value) {
            vm.temperature_to_value = vm.convertFahrenheitToCelcius(vm.temperature_from_value);
          } else {
            vm.temperature_from_value = vm.convertCelciusToFahrenheit(vm.temperature_to_value);
          }
        } else {
          if (vm.temperature_from_value) {
            vm.temperature_to_value = vm.convertCelciusToFahrenheit(vm.temperature_from_value);
          } else {
            vm.temperature_from_value = vm.convertFahrenheitToCelcius(vm.temperature_to_value);
          }
        }
      },

      convertFahrenheitToCelcius: function(value) {
        return ((parseFloat(value) - 32) * 5.0 / 9.0).toFixed(1);
      },

      convertCelciusToFahrenheit: function(value) {
        return (32 + parseFloat(value) * 9.0 / 5.0).toFixed(1);
      },

    },

    watch: {
    },
  }
</script>
