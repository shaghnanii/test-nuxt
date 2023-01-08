<template>
  <b-card-group deck>
    <b-card
      header="Store Current Status"
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
</template>

<script>
import {mapActions} from "vuex";

export default {
  name: "OpenCloseComponent",
  data() {
    return {
      isLoading: true,
      data: {
        status: '',
        message: '',
      },
    }
  },
  async created() {
    await this.getDetails();
  },
  methods: {
    ...mapActions('global', ['index']),

    async getDetails() {
      const response = await this.index(`/calculate`);
      if (response.status === 200) {
        this.data.status = "Store is open"
        this.data.message = response.data.message;
      } else if (response.status === 404) {
        this.data.status = "Store is close";
        this.data.message = response.data.message;
      } else {
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
