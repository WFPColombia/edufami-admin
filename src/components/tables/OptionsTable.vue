<template>
  <v-card>
    <v-card-text class="pa-0">
      <template>
        <v-data-table :headers="headers" :items="stepOptions" hide-actions class="elevation-0">
          <template slot="items" slot-scope="scope">
            <td v-for="(i,key) in headers" :key="key">
              <router-link
                v-if="i.value === 'question'"
                :to="`/step/${scope.item.id}`"
              >{{scope.item[i.value]}}</router-link>
              <span v-else>{{scope.item[i.value]}}</span>
            </td>
          </template>
        </v-data-table>
      </template>
    </v-card-text>
  </v-card>
</template>
<script>
import { mapGetters } from "vuex";
export default {
  components: {},
  props: {},
  data() {
    return {
      headers: [],
      items: []
    };
  },
  beforeMount() {
    this.headers = [
      // { text: "Order", value: "order" },
      { text: "Text", value: "text" },
      { text: "Is correct?", value: "isCorrect" },
      { text: "Feedback", value: "feedback" }
      // { text: "Column", value: "column" }
    ];
  },
  computed: {
    ...mapGetters(["stepOptions"])
  },
  methods: {}
};
</script>
