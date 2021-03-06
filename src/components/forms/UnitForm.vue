<template>
  <v-layout row justify-center>
    <v-btn primary color="secondary" @click="dialog = true" small>{{this.label}}</v-btn>
    <v-dialog v-model="dialog" max-width="600px">
      <v-card>
        <v-form ref="form" v-model="valid" lazy-validation>
          <v-card-title>
            <span class="headline">{{this.label}}</span>
          </v-card-title>
          <v-card-text>
            <v-container grid-list-md>
              <v-layout wrap>
                <v-flex xs12 v-for="(item, key) in form" :key="key">
                  <v-text-field
                    :label="item.label"
                    :hint="item.hint"
                    :counter="item.counter"
                    :required="item.required"
                    :rules="item.rules"
                    v-model="formData[item.model]"
                  />
                </v-flex>
              </v-layout>
            </v-container>
            <small>*indicates required field</small>
          </v-card-text>
          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn color="secondary" dark @click="handleClose">Close</v-btn>
            <v-btn color="primary" dark @click="handleSave">Save</v-btn>
          </v-card-actions>
        </v-form>
      </v-card>
    </v-dialog>
  </v-layout>
</template>
<script>
import { mapActions } from "vuex";

export default {
  props: {
    label: String,
    training: Object,
    data: {
      type: Object,
      default: () => {
        return {};
      }
    }
  },
  data: () => ({
    dialog: false,
    valid: false,
    form: [
      {
        model: "name",
        type: "text",
        label: "Unit Name",
        hint: "The name of the unit that the user can read",
        counter: 60,
        required: true,
        rules: [
          v => !!v || "Unit name is required",
          v => (v && v.length <= 60) || "Name must be less than 60 characters"
        ]
      },
      {
        model: "description",
        type: "text",
        label: "Description",
        hint: "A short description that explains the content of the unit",
        counter: 160,
        required: true,
        rules: [
          v => !!v || "The Unit's description is required",
          v =>
            (v && v.length <= 160) ||
            "Description must be less than 160 characters"
        ]
      },
      {
        model: "image",
        type: "text",
        label: "Image",
        hint: ""
      },
      {
        model: "objective",
        type: "text",
        label: "objective",
        hint: ""
      },
      {
        model: "order",
        type: "text",
        label: "Order",
        hint: ""
      }
    ],
    formData: {}
  }),
  computed: {},
  methods: {
    ...mapActions(["postUnitbyTraining", "patchUnit"]),
    handleSave() {
      if (this.$refs.form.validate()) {
        if (this.formData.id) {
          this.patchUnit(this.formData).then(() => {
            this.dialog = false;
          });
        } else {
          this.postUnitbyTraining({
            trainingId: this.training.id,
            data: this.formData
          }).then(() => {
            this.$refs.form.reset();
            this.dialog = false;
          });
        }
      }
    }
  },
  mounted() {
    this.formData = { ...this.data };
  }
};
</script>

