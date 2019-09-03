<template>
  <div class="container p-0">
    <div>
      <Navbar />
    </div>
    <!-- main container -->
    <div>
      <h1 class="text-center text-white mt-5">Select Cribs</h1>
      <div class="p-ud d-none d-sm-block">
        <ul class="container_list text-center">
          <li>
            <b-img v-bind="mainProps" blankColor="#B3BD27" rounded="circle" alt="Circle image"></b-img>
            <span class="d-block">Select Crib</span>
          </li>
          <li>
            <b-img v-bind="mainProps" rounded="circle" alt="Circle image"></b-img>
            <span class="d-block">Select Date</span>
          </li>
          <li>
            <b-img v-bind="mainProps" rounded="circle" alt="Circle image"></b-img>
            <span class="d-block">Your Details</span>
          </li>
          <li>
            <b-img v-bind="mainProps" rounded="circle" alt="Circle image"></b-img>
            <span class="d-block">Confirm</span>
          </li>
        </ul>
      </div>
    </div>
    <!-- dropdowns -->
    <div class="p-ud">
      <b-row class="text-white" no-gutters>
        <b-col sm="6" md="4" class="p-1">
          <b-form-select
            class="custom-select"
            v-model="selectedCity"
            :options="cityOptions"
            @change="fetchBedroom"
          ></b-form-select>
        </b-col>
        <b-col sm="6" md="4" class="p-1">
          <b-form-select v-model="selectedBedroom" :options="bedroomOptions"></b-form-select>
        </b-col>
        <b-col sm="6" md="4" class="p-1">
          <b-form-select v-model="selected" :options="options"></b-form-select>
        </b-col>
      </b-row>
    </div>
    <!-- boxes -->
    <div class="p-ud">
      <b-row class="text-white" no-gutters>
        <b-col>
          <h4 class="text-center p-ud">Houses List</h4>
          <div class="list-container">
            <b-list-group class="list-background">
              <b-list-group-item class="d-flex justify-content-between align-items-center">
                Cras justo odio
                <b-badge variant="primary" pill>14</b-badge>
              </b-list-group-item>

              <b-list-group-item class="d-flex justify-content-between align-items-center">
                Dapibus ac facilisis in
                <b-badge variant="primary" pill>2</b-badge>
              </b-list-group-item>

              <b-list-group-item class="d-flex justify-content-between align-items-center">
                Morbi leo risus
                <b-badge variant="primary" pill>1</b-badge>
              </b-list-group-item>
            </b-list-group>
          </div>
        </b-col>
        <b-col class="d-none d-sm-block pl-2">
          <h4 class="text-center p-ud">Houses on Map</h4>
          <div>
            <img src="../assets/maps.jpg" />
          </div>
        </b-col>
      </b-row>
    </div>
    <div>
      <b-button class="d-flex flex-row-reverse btn-success" variant="success">NEXT</b-button>
    </div>
    <!-- footer -->
    <div class="text-muted text-center p-footer">
      <h6>Student Cribs &#169; 2019</h6>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import Navbar from "@/components/Navbar.vue";
import axios from "axios";

export default {
  name: "home",
  components: {
    Navbar
  },
  mounted() {
    this.fetchCityList();
  },
  data() {
    return {
      baseUrl: "https://api.student-cribs.com/api",
      auth:
        "auth=504a9444668c2a591ebc178c832f028ee6bd14a2e2716a7fbd7b4e94ecb9b09a240af1fe74290c63922b3c5591d565c5a8768bfecea5324e707355c5dee7cf70",
      urlCity: "/cities/?",
      // city vars
      selectedCity: null,
      cityOptions: [],
      // bedroom vars
      selectedBedroom: null,
      bedroomOptions: [
        {
          value: null,
          text: "No. of bedrooms"
        }
      ],
      // sort vars
      selected: "a",
      options: [
        { value: "a", text: "Sort by: Lowest price" },
        { value: "b", text: "Sort by: Highest price" }
      ],
      mainProps: {
        blank: true,
        blankColor: "#D8D8D8",
        width: 40,
        height: 40
      }
    };
  },
  methods: {
    fetchCityList() {
      this.cityOptions = [];
      this.cityOptions.push({ value: null, text: "Select city" });
      axios
        .get(`${this.baseUrl}${this.urlCity}${this.auth}`)
        .then(res => {
          res.data.data.forEach(element => {
            this.cityOptions.push({
              value: element.slug,
              text: element.name,
              url: element.url
            });
          });
        })
        .catch(err => {
          console.log(err);
        });
    },
    fetchBedroom(e) {
      if (!e) {
        return;
      }
      this.bedroomOptions = [];
      this.bedroomOptions.push({
        value: null,
        text: "No. of bedrooms"
      });
      axios
        .get(`${this.baseUrl}/cities/slug/${e}/?${this.auth}`)
        .then(res => {
          res.data.data.bedrooms.forEach(element => {
            this.bedroomOptions.push({
              value: element.bedrooms_amount,
              text: element.bedrooms_amount + " bedrooms"
            });
          });
        })
        .catch(err => {
          console.log(err);
        });
    }
  }
};
</script>
<style lang="css" src="./Home.css" scoped>
</style>
