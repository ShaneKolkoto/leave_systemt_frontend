<template>
  <v-container fluid>
    <v-form
      ref="form"
      v-model="valid"
      lazy-validation
      class="elevation-5 rounded-lg px-5 py-7 mt-5"
      height="200"
    >
      <v-row>
        <v-col cols="12">
          <div class="headline">Personal</div>
        </v-col>
        <v-col cols="12" md="6" class="mb-0">
          <v-text-field
            v-model="formData.firstName"
            label="First Name"
            outlined
            prepend-inner-icon="mdi-account-arrow-right-outline"
            :rules="requiredRules"
          />
        </v-col>
        <v-col cols="12" md="6">
          <v-text-field
            v-model="formData.lastName"
            label="Last Name"
            outlined
            prepend-inner-icon="mdi-account-arrow-left-outline"
            :rules="requiredRules"
          />
        </v-col>
        <v-col cols="12" md="6">
          <v-text-field
            @input="getUser(formData.email)"
            v-model="formData.email"
            label="E-Mail"
            outlined
            :rules="emailRules"
            prepend-inner-icon="mdi-email-outline"
          />
        </v-col>
        <v-col cols="12" md="6">
          <v-text-field
            v-model="formData.contact"
            label="Phone Number"
            outlined
            counter="10"
            :rules="numberRules"
            prepend-inner-icon="mdi-cellphone"
          />
        </v-col>
      </v-row>
      <v-col cols="12">
        <v-autocomplete
          :items="leaveType"
          label="Leave Type"
          :rules="requiredRules"
          v-model="formData.leave_type"
        />
      </v-col>
      <v-row class="mb-o">
        <v-col cols="12" md="6">
          Starting Date:
          <input class="ml-3" type="date" v-model="formData.start_date" />
        </v-col>
        <v-col cols="12" md="6">
          Ending Date:
          <input type="date" v-model="formData.end_date" />
        </v-col>
      </v-row>
      <v-row class="mb-0">
        <v-col cols="12">
          <v-text-field
            v-model="formData.reason"
            label="Reason"
            outlined
            :rules="requiredRules"
            prepend-inner-icon="mdi-notebook"
          />
        </v-col>
      </v-row>

      <v-row>
        <v-col cols="12">
          <v-btn color="primary" @click="validate"> Submit </v-btn>
        </v-col>
      </v-row>
    </v-form>

    <UserInfo :user="user" />
  </v-container>
</template>

<script>
import UserInfo from "@/components/UserInfo.vue";

export default {
  data: () => ({
    valid: false,
    calenderModal: false,
    formLoading: false,
    formData: {},
    user: null,
    dates: {
      start_date: "",
      end_date: "",
    },
    leaveType: ["Sick Leave", "Family Responsibilities", "Study Leave"],
    requiredRules: [(v) => !!v || "Please fill out this field!"],
    numberRules: [
      (v) => !!v || "Please fill out this field!",
      (v) => Number.isInteger(Number(v)) || "Please enter numbers only!",
    ],
    emailRules: [
      (v) => !!v || "Please fill out the field!",
      (v) =>
        !v ||
        /^\w+([.-]?\w+)*@\w+([.-]?\w+)*(\.\w{2,3})+$/.test(v) ||
        "E-mail must be valid",
    ],
  }),
  methods: {
    validate() {
      if (this.$refs.form.validate()) {
        this.formLoading = true;
        fetch("http://localhost:3000/apply-leave", {
          method: "POST",
          headers: {
            "Content-Type": "application/json",
            // 'Content-Type': 'application/x-www-form-urlencoded',
          },
          body: JSON.stringify({
            leave_type: this.formData.leave_type,
            start_date: this.formData.start_date,
            end_date: this.formData.end_date,
            name: this.formData.firstName,
            surname: this.formData.lastName,
            email: this.formData.email,
            contact: this.formData.contact,
          }),
        })
          .then((response) => response.json())
          .then((data) => {
            if (data.status === 200) {
              alert(data.data);
              this.formLoading = false;
              this.reset();
              this.user = null;
            }
          });
      }
    },
    reset() {
      this.$refs.form.reset();
    },
    validateDate(date) {
      console.log(date);
    },
    /* eslint-disable */

    validateEmail(value) {
      if (/^\w+([\.-]?\w+)*@\w+([\.-]?\w+)*(\.\w{2,3})+$/.test(value)) {
        return true;
      } else {
        return false;
      }
    },

    async getUser(email) {
      if (this.validateEmail(email) === true) {
        await fetch("http://localhost:3000/employee/find-user", {
          method: "POST",
          headers: {
            "Content-Type": "application/json",
            // 'Content-Type': 'application/x-www-form-urlencoded',
          },
          body: JSON.stringify({
            email: email,
          }),
        })
          .then((response) => response.json())
          .then((data) => {
            if (data.status === 200) {
              this.user = data.data;
            }
          });
      }
    },
  },
  //   computed: {
  //     theme() {
  //       return this.$vuetify.theme.dark ? "dark" : "light";
  //     },
  //   },
  components: {
    UserInfo,
  },
};
</script>

<style>
form {
  width: 80%;
  margin: 0 auto;
  background-color: #eeeeee;
}
</style>
