<template>
  <div>
    <v-container>
      <v-row>
        <v-col>
          <v-card>
            <v-card-title> <h3>Current Temp Madrid :</h3>  {{ madrid }} C </v-card-title>
          </v-card>
        </v-col>
        <v-spacer />
        <v-col>
          <v-card>
            <v-card-title> <h3>Current Temp Barcelona :</h3>  {{ barcelona }} C </v-card-title>
          </v-card>
        </v-col>
        <br>
      </v-row>
    </v-container>
    <br>
    <v-row>
      <v-card>
        <v-card-title>
          <v-text-field
            v-model="search"
            append-icon="mdi-magnify"
            label="Search"
            single-line
            hide-details
          />
        </v-card-title>
        <v-data-table
          :headers="headers"
          :items="desserts"
          :search="search"
          class="elevation-1"
        >
          <template v-slot:[`item.status`]="{ item }">
            <v-chip
              :color="getColor(item.status)"
              dark
            >
              {{ item.status }}
            </v-chip>
          </template>
        </v-data-table>
      </v-card>
    </v-row>
    <br>
    <br>
    <br>
  </div>
</template>

<script>
export default {
  data () {
    return {
      search: '',
      headers: [
        { text: 'City', align: 'start', filterable: true, value: 'city_name' },
        { text: 'Description', value: 'description', filterable: false },
        { text: 'Lon', value: 'lon', filterable: false },
        { text: 'Lat', value: 'lat', filterable: false },
        { text: 'Wind_speed', value: 'wind_speed', filterable: false },
        { text: 'Humidity', value: 'humidity', filterable: false },
        { text: 'Pressure', value: 'pressure', filterable: false },
        { text: 'Temp', value: 'temp', filterable: false },
        { text: 'Temp-Max', value: 'temp_max', filterable: false },
        { text: 'Temp-Min', value: 'temp_min', filterable: false },
        { text: 'Status', value: 'status', filterable: false },
        { text: 'Date', value: 'created_at', filterable: true }
      ],
      desserts: [],
      madrid: '',
      barcelona: ''
    }
  },
  async fetch () {
    this.desserts = await fetch(
      'http://localhost:8000/api/dbfetch'
    ).then(res => res.json())
    this.barcelona = await fetch(
      'http://localhost:8000/api/barcelona'
    ).then(res => res.json())
    this.madrid = await fetch('http://localhost:8000/api/madrid').then(res =>
      res.json()
    )
  },
  methods: {
    getColor (status) {
      if (status === 'hot Weather') {
        return 'red'
      } else if (status === 'cold weather') {
        return 'red'
      }
      // good weather
      else {
        return 'blue'
      }
    }
  },
  // call fetch only on client-side
  fetchOnServer: false
}
</script>
