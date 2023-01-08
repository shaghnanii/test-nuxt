<template>
  <div>

    <b-form @submit="checkDay" inline class="mb-2">
      <b-form-datepicker id="example-datepicker" v-model="selectedDate" class="mr-4"></b-form-datepicker>
      <b-button type="submit" variant="primary">Search</b-button>
    </b-form>

    <b-card-group deck v-if="isShow">
      <b-card
        header="Choose a date"
        header-tag="header"
      >
        <template v-if="isLoading">
          <CardSkeleton />
        </template>
        <template v-else>
          <b-card-title>{{ data.status }}</b-card-title>
          <b-card-text>{{ data.message }}</b-card-text>
        </template>
      </b-card>
    </b-card-group>
  </div>
</template>

<script>
import {mapActions} from "vuex";

export default {
  name: "DatePickerComponent",
  data() {
    return {
      isLoading: true,
      isShow: false,
      selectedDate: '',
      errors: [],
      data: {
        status: '',
        message: '',
      },
    }
  },
  methods: {
    ...mapActions('global', ['store']),

    async checkDay(event) {
      this.isShow = true;
      event.preventDefault()
      const payload = {
        url: '/calculate',
        data: {
          selected_date: this.selectedDate
        }
      }
      const response = await this.store(payload);
      if (response.status === 200) {
        this.data.status = "Store is open"
        this.data.message = response.data.message;
      } else if (response.status === 404) {
        this.data.status = "Store is close";
        this.data.message = response.data.message;
      }
      else if(response.status === 422) {
        this.data.status = "Validation Error";
        this.data.message = response.data.message
      }
      else {
        console.log("Server Error")
        this.data.status = "Server Error"
        this.data.message = "Network is not properly connected or something went wrong. Please contact your admin."
      }
      this.isLoading = false;
    }
  }
}
</script>

<style scoped>

</style>
