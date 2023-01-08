<template>
  <div>
    <b-container class="bv-example-row mt-5">
      <b-row>
        <b-col cols="12" md="12">
          <div>
<!--            Component/Widget to check weather the shop is open or closed at the moment.-->
            <OpenCloseComponent />
          </div>
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import {mapActions} from "vuex";
import OpenCloseComponent from "~/components/Widgets/OpenCloseComponent";

export default {
  name: 'IndexPage',
  components: {OpenCloseComponent},
  data() {
    return {}
  },
}
</script>
