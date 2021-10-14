<template>
  <div class="container">
    <form @submit.prevent="submit" autocomplete="off">
      <label for="icao"><h3 class="fs-2">Enter ICAO code</h3></label>
      <div class="input-group">
        <input
          type="text"
          v-model="icao"
          minlength="4"
          maxlength="4"
          class="form-control"
          placeholder="ICAO code"
          id="icao"
          required
        />
        <button @click="getMETAR()" class="btn btn-success">get METAR</button>
      </div>
    </form>
    <div class="p-3 bg-dark rounded my-3">
      <strong>{{ metarTitle }}</strong>
      <p class="text-break font-monospace">{{ metar }}</p>
    </div>
    <div class="p-3 bg-dark rounded mb-4">
      <strong>{{ detailsTitle }}</strong>
      <section style="text-align: left !important">
        <p v-for="detail in details" :key="detail.key">
          <span>{{ detail.key }}: </span>{{ detail.description }}
        </p>
      </section>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "Header",
  data() {
    return {
      icao: "",
      metarTitle: "METAR:",
      detailsTitle: "Details:",
      metar: "",
      details: [],
    };
  },
  methods: {
    getMETAR: function() {
      axios
        .get(
          `https://api.checkwx.com/metar/${this.icao.toUpperCase()}/decoded?x-api-key=d3306b6cf7674987a7b96c622d`
        )
        .then((response) => {
          console.log(response.data);
          if (response.data.results == 0) {
            this.metar = `${this.icao} airport was not found`;
          } else {
            const res = response.data.data[0];
            this.metar = res.raw_text;
            this.metarTitle = `METAR for ${this.icao.toUpperCase()}:`;
            this.detailsTitle = `Details for ${this.icao.toUpperCase()}:`;
            this.details = [
              {
                key: "flight category",
                description: res.flight_category,
              },
              {
                key: "wind",
                description: `${res.wind.degrees}° at ${res.wind.speed_kts}knots`,
              },
              {
                key: "visibility",
                description: `${res.visibility.miles_float}miles / ${res.visibility.meters_float}meters`,
              },
              {
                key: "clouds",
                description: res.clouds[0].text,
              },
              {
                key: "temperature",
                description: `${res.temperature.celsius}°C / ${res.temperature.fahrenheit}°F`,
              },
              {
                key: "dewpoint",
                description: `${res.dewpoint.celsius}°C / ${res.dewpoint.fahrenheit}°F`,
              },
              {
                key: "humidity",
                description: `${res.humidity.percent}%`,
              },
              {
                key: "pressure",
                description: `${res.barometer.hpa}hPa`,
              },
              {
                key: "elevation",
                description: `${res.elevation.feet}feet / ${res.elevation.meters}meters`,
              },
            ];
          }
        })
        .catch((error) => {
          console.log(error);
          this.metar = `Error`;
        });
    },
  },
};
</script>
