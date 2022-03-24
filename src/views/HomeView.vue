<template>
<v-container>
  <div>
    <div class="form">
    <v-text-field
      class="input"
      label="Unesite ime"
      v-model="name"
    ></v-text-field>
    <!-- <input @click="test()" type="submit" /> -->
    <v-btn class="button" @click="test()">gumb</v-btn>
  </div>
    <div>

      <v-data-table
        :headers="headers"
        :items="user_data"
        class="elevation-1"
      ></v-data-table>
    </div>
  </div>
  </v-container>
</template>

<script>
import axios from "axios";

export default {
  name: "Home-View",

  data() {
    return {
      headers: [
        {
          text: "Ime",
          align: "start",
          sortable: false,
          value: "name",
        },
        { text: "Godine", value: "godine" },
        { text: "Zemlja", value: "zemlja" },
        { text: "Spol", value: "spol" },
      ],
      user_data: [],
      name: "",
      podaci: [],
      spol: "",
      godine: "",
      zemlja: [],
    };
  },
  methods: {
    async test() {
      let get_godine = await axios.get(
        `https://api.agify.io/?name=${this.name}`
      );
      this.godine = get_godine.data.age;

      let get_zemlja = await axios.get(
        `https://api.nationalize.io/?name=${this.name}`
      );
      this.zemlja = get_zemlja.data;
      console.log(get_zemlja.data);


      let get_spol = await axios.get(
        `https://api.genderize.io/?name=${this.name}`
      );
      this.spol = get_spol.data;
      console.log(get_spol.data.gender);

      let z = "";
      this.zemlja.country.forEach((drzava) => {
        z =
          z +
          " " +
          drzava.country_id +
          " (" +
          (drzava.probability * 100).toFixed(2) +
          "%), ";
      });

      console.log(z);

      this.user_data.push({
        name: this.name,
        godine: this.godine,
        zemlja: z,
        spol: this.spol.gender + " (" + (this.spol.probability*100).toFixed(2) + "%)",
      });
      console.log(this.zemlja.country.length);
    },
  },
  async mounted() {},
};
</script>

<style scoped>
.input {
  width: 200px;

}
.form{
  align-items: center;
  display: flex;
  flex-direction: row;
  width: 300px;
  margin: 30px 0 0 30px;
}
.button{
  margin-left: 20px;
}
</style>
