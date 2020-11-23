<template>
  <div>
    <!-- start upload-->
    <v-container fluid class="d-flex justify-center">
      <v-card elevation="24">
        <form
          enctype="multipart/form-data"
          @submit.prevent="sendFile"
          class="vcardstyle"
        >
          <v-col>
            <v-textarea
              auto-grow
              v-model="comment"
              label="Comment"
              rows="1"
              prepend-icon="mdi-comment"
              required
            ></v-textarea>
          </v-col>
          <v-row>
            <div class="d-flex justify-center">
              <v-col cols="4" class="pa-4">
                <v-subheader>Invoice total amount</v-subheader>
              </v-col>
              <v-col cols="8" class="pr-10">
                <v-text-field
                  v-model="amount"
                  label="Amount"
                  placeholder="10.00"
                  value=""
                  prefix="$"
                  required
                ></v-text-field>
              </v-col>
            </div>
          </v-row>
          <div class="d-flex justify-center pa-4">
            <input type="file" ref="file" @change="selectFile" required />
            <v-btn type="submit" color="blue-grey" class="ma-2 white--text">
              Upload
              <v-icon right dark> mdi-cloud-upload </v-icon>
            </v-btn>
          </div>
          <v-progress-linear
            v-if="progressbar"
            indeterminate
            color="blue"
          ></v-progress-linear>
        </form>
      </v-card>
    </v-container>
    <!-- end upload-->

    <!-- start data-->
    <v-container>
      <!-- <vcardimage :comment="com" image='ima' amount="15.00"></vcardimage> -->
      <vcardimage
        v-for="(item, index) in invoices"
        :key="index"
        :comment="item.comment"
        :image="item.file"
        :amount="item.amount"
        class="vcardstyle velements"
      ></vcardimage>
    </v-container>
    <!-- end data-->
  </div>
</template>

<script>
import axios from "axios";
import vcardimage from "./vcardimage";

export default {
  name: "inputform",
  components: {
    vcardimage,
  },
  data() {
    return {
      progressbar: false,
      show: false,
      file: "",
      message: "",
      error: false,
      amount: "",
      comment: "",
      invoices: "",
    };
  },
  methods: {
    reloadinvoices() {
      axios.get("https://recibos.fredy.dev/invoices").then((response) => {
        console.log(response.data);
        this.invoices = response.data;
      });
    },
    selectFile() {
      this.file = this.$refs.file.files[0];
      this.error = false;
      this.message = "";
    },
    async sendFile() {
      this.progressbar = true;
      const formData = new FormData();
      formData.append("comment", this.comment);
      formData.append("amount", this.amount);
      formData.append("file", this.file);

      try {
        await axios
          .post("https://recibos.fredy.dev/upload", formData)
          .then(function (response) {
            // handle success
            console.log(response);
            
          })
          .catch(function (error) {
            // handle error
            console.log(error);
          });
        console.log(formData);
        this.message = "file has been update";

        setTimeout(() => {
          this.progressbar = false;
          this.reloadinvoices();
        }, 1500);
        this.file = "";
        this.comment = "";
        this.amount = "";
        this.error = false;
      } catch (err) {
        console.log(err);
        this.message = "something went wrong";
        this.error = true;
      }
    },
  },
  mounted() {
    this.reloadinvoices();
  },
};
</script>

<style scoped>
/* * {
  border: 1px solid black;
} */
.vcardstyle {
  border: 2px solid gray;
}
.velements {
  margin-bottom: 15px;
}
</style>