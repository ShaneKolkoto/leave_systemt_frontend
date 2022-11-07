<template class="white--text">
  <v-row class="px-2 py-2 mt-3" v-if="user">
    <v-col cols="12" md="6">
      <v-card max-width="450px" class="mx-auto bg" elevation="2">
        <v-row justify="center">
          <v-col
            align-self="start"
            class="d-flex justify-center align-center pa-0 mt-3"
            cols="12"
          >
            <v-avatar
              class="profile avatar-center-heigth avatar-shadow"
              color="grey"
              size="164"
            >
              <v-img
                src="https://cdn.vuetifyjs.com/images/profiles/marcus.jpg"
              ></v-img>
            </v-avatar>
          </v-col>
        </v-row>
        <v-list-item color="#0000" class="profile-text-name ma-4 pt-16">
          <v-list-item-content>
            <v-list-item-title class="text-h6">
              {{ user[0].name }} {{ user[0].surname }}
            </v-list-item-title>
            <v-list-item-subtitle>{{ user[0].email }}</v-list-item-subtitle>
            <v-list-item-subtitle
              >Amount of leave days:
              <span class="font-weight-bold">
                {{ user[user.length - 1].amount_leave_days }}
              </span></v-list-item-subtitle
            >
          </v-list-item-content>
        </v-list-item>
      </v-card>
    </v-col>

    <v-col cols="12" md="6">
      <v-row><h4 class="white--text text-center mb-2">History</h4></v-row>
      <v-row>
        <table class="table table-bordered" id="history">
          <thead>
            <tr>
              <th>#</th>
              <th>Start Dates</th>
              <th>End Dates</th>
              <th>Leave Type</th>
              <th>Total Amount of days</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(leave, index) in user" :key="index">
              <td>{{ index + 1 }}</td>
              <td>{{ moment(leave.start_date).format("YYYY-MM-DD") }}</td>
              <td>{{ moment(leave.end_date).format("YYYY-MM-DD") }}</td>
              <td>{{ leave.leave_type }}</td>
              <td>{{ leave.amount_of_days }}</td>
            </tr>
          </tbody>
        </table>
      </v-row>
    </v-col>
  </v-row>
</template>

<script>
export default {
  props: ["user"],
  data() {
    return {
      show: true,
    };
  },
  watch: {
    // whenever question changes, this function will run
    form() {
      if (!this.form.email) {
        this.show = false;
      } else {
        if (this.validateEmail(this.form.email) !== false) {
          this.show = true;
        }
      }
    },
  },
};
</script>

<style scoped>
#history {
  font-family: Arial, Helvetica, sans-serif;
  border-collapse: collapse;
  width: 80%;
}

#history tr:hover {
  color: #ddd;
  background-color: #111111;
}
#history tr {
  color: #111111;
  background-color: #ddd;
}

#history th {
  padding-top: 12px;
  padding-bottom: 12px;
  text-align: left;
  background-color: #04aa6d;
  color: white;
}
</style>
