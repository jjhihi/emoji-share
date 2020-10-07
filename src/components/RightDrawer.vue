<template>
<v-container>
      <v-subheader> 평점 </v-subheader>
      <v-row align="center" no-gutters v-for="n in rating" :key="n">
        <v-checkbox v-model="enabled[n]" hide-details style="margin-top: 0px; display: inline-block;"></v-checkbox>
        <v-rating length="5" size="20" :value="n" readonly style="display: inline-block;"/>
      </v-row>
<v-divider></v-divider>
<v-row no-gutters>
<v-spacer> </v-spacer>
<v-btn color="primary">Apply</v-btn>
<v-btn color="error">Reset</v-btn>
</v-row>
    </v-container>
</template>

<script>
  export default {
    name: 'RightDrawer',

    components: {
    },

    data: () => ({
enabled: [false, false, false, false, false],
rating: [5,4,3,2,1],
alignments: [
        'start',
        'center',
        'end',
      ],
    }),
    methods: {
    },
    mounted() {
    },
    beforeCreate(){
    },
    created() {
    },
    updated() {
    }
  }
</script>
