<template>
  <v-container class="my-16">
    <v-alert v-model="alert.status" type="info" variant="tonal" dismissible>
      {{ alert.text }}
    </v-alert>
    <title-banner
      :title="titleData.title"
      :description="titleData.description"
      :disclaimer="titleData.disclaimer"
    />

    <v-row class="mt-10">
      <v-col cols="12" md="6" class="text-body-1 pr-md-16">
        <h2 class="text-h5 text-md-h4 font-weight-medium">Drivers</h2>
        <p class="mt-2">Upload a file with the names of your drivers</p>
        <v-file-input
          v-model="driversFile"
          accept=".txt"
          label="Click here to select a .txt file"
          density="compact"
          show-size
          variant="underlined"
          prepend-icon="mdi-human-scooter"
          @change="importDriversTxt('drivers')"
        />
        <v-textarea v-model="drivers.names" rows="5" no-resize disabled />
        <span> {{ drivers.quantity }} drivers loaded. </span>
      </v-col>

      <v-col cols="12" md="6" class="text-body-1 pl-md-16">
        <h2 class="text-h5 text-md-h4 font-weight-medium">Shipment</h2>
        <p class="mt-2">Upload a file with you shipments</p>
        <v-file-input
          v-model="shipmentsFile"
          accept=".txt"
          label="Click here to select a .txt file"
          density="compact"
          variant="underlined"
          show-size
          prepend-icon="mdi-truck-fast"
          @change="importDriversTxt('shipments')"
        />
        <v-textarea v-model="shipments.names" rows="5" no-resize disabled />
        <span> {{ shipments.quantity }} shipments loaded. </span>
      </v-col>
    </v-row>

    <div class="d-flex justify-center mt-16">
      <v-btn color="primary" large depressed @click="calculateShipments()">Calculate</v-btn>
    </div>
  </v-container>
</template>

<script>
import TitleBanner from '@/components/TitleBanner.vue'

export default {
  name: 'ShipmentsDestination',
  components: {
    TitleBanner,
  },

  data: () => ({
    alert: {
      status: false,
      text: '',
    },
    titleData: {
      title: 'Assigns shipment destination',
      description:
        'Optimize your logistics levels by efficiently assigning drivers to their delivery routes. Upload your list of drivers in a .txt file separating with a line break for each driver. for each driver. It is also necessary to upload your list of delivery addresses in a .txt file with a line break for each driver. .txt file with a line break for each address.',
      disclaimer:
        '1. the number of drivers must match the number of delivery addresses. 2. Clic calculate to download a .csv',
    },
    driversFile: null,
    shipmentsFile: null,
    scores: [],
    results: [],
    driverSuitability: null,
    drivers: {
      names: '',
      quantity: 0,
      array: {},
    },
    shipments: {
      names: '',
      quantity: 0,
      array: {},
    },
  }),
  methods: {
    /**
     * Take the information that comes inside the .txt files.
     * @param type - flag to differentiate whether the file belongs to the driver or to the shipment.
     * @returns {array} - array with driver names or addresses
     **/
    importDriversTxt(type) {
      let reader = new FileReader()
      //Read if drivers file is upload
      if (type === 'drivers') {
        reader.readAsText(this.driversFile)
        //take the info and convert in array when exist and \n or \r
        reader.onload = () => {
          this.drivers.names = reader.result
          this.drivers.quantity = this.drivers.names.split(/\r\n|\r|\n/).length
          this.filterStringData(type)
        }
      } else {
        //Read if shipments file is upload
        reader.readAsText(this.shipmentsFile)
        reader.onload = () => {
          this.shipments.names = reader.result
          this.shipments.quantity = this.shipments.names.split(/\r\n|\r|\n/).length
          this.filterStringData(type)
        }
      }
    },
    /**
     * Cleans arrays containing '\r' and '\n'.
     * @param type - flag to differentiate whether the file belongs to the driver or to the shipment.
     * @returns {array} - array with driver names or addresses free of '\r' and '\n'
     **/
    filterStringData(type) {
      if (type === 'drivers') {
        this.drivers.names = this.drivers.names.replaceAll('\r', '')
        this.drivers.array = this.drivers.names.split('\n')
      } else {
        this.shipments.names = this.shipments.names.replaceAll('\r', '')
        this.shipments.array = this.shipments.names.split('\n')
      }
    },
    /**
     * Defines if a number is odd or even.
     * @param number - receives a character length
     * @returns {number} - is odd = 1 , even = 0
     **/
    isOdd(number) {
      return !(number % 2)
    },
    /**
     * Counts the number of consonants in a string.
     * @param driverName - receives the driver name in a string variable
     * @returns {number} - returns the number of consonants
     **/
    countConsonants(driverName) {
      const match = driverName
        .toLowerCase()
        .replaceAll(' ', '')
        .match(/[^aeiou]/gi)
      return match !== null ? match.length : 0
    },
    /**
     * Counts the number of vowels in a string.
     * @param driverName - receives the driver name in a string variable
     * @returns {number} - returns the number of vowels
     **/
    countVowels(driverName) {
      const match = driverName
        .toLowerCase()
        .replaceAll(' ', '')
        .match(/[aeiou]/gi)
      return match !== null ? match.length : 0
    },
    /**
     * Take the drivers' names and addresses and compare if they are a match
     * (a match is the length of the shipment's destination street name shares
     * any common factors (besides 1) with the length of the driver’s name)
     * @param address - receives the address name in a string variable
     * @param name - receives the driver name in a string variable
     * @returns {Boolean} - returns true if have a match
     **/
    matchCondition(address, name) {
      let addressMatches = []
      let nameMatches = []
      //the index starts at 2 because we ignore if the match have a common factors with de number 1
      for (let i = 2; i <= address; i++) {
        if (address % i === 0) {
          addressMatches.push(i)
        }
      }
      for (let j = 2; j <= name; j++) {
        if (name % j === 0) {
          nameMatches.push(j)
        }
      }
      return addressMatches.filter((value) => nameMatches.includes(value)).length > 0
    },
    /**
     * converts an array into a csv document by taking the keys as titles
     **/
    exportToCsv() {
      // take the titles of the keys of the arrangement
      const titleKeys = Object.keys(this.results[0])
      const refinedData = []
      refinedData.push(titleKeys)

      this.results.forEach((item) => {
        refinedData.push(Object.values(item))
      })

      let csvContent = ''

      refinedData.forEach((row) => {
        csvContent += row.join(',') + '\n'
      })
      //converts the information into a blob so that the file can be downloaded.
      const blob = new Blob([csvContent], { type: 'text/csv;charset=utf-8,' })
      const objUrl = URL.createObjectURL(blob)
      const link = document.createElement('a')
      link.setAttribute('href', objUrl)
      link.setAttribute('download', 'Results.csv')
      document.querySelector('body').append(link)
      link.click()
    },

    calculateShipments() {
      // Validate correct data length.
      if (this.shipments.array.length !== this.drivers.array.length) {
        this.alert.status = true
        this.alert.text =
          'The number of drivers must be the same as the number of addresses. Please try again'
        return
      }

      this.shipments.array.forEach((address) => {
        if (!this.isOdd(address.length)) {
          // Even case: SS = the number of vowels in the driver’s name multiplied by 1.5
          this.driverSuitability = this.drivers.array.map(
            (driver) => this.countVowels(driver) * 1.5
          )
          // If matches exists the SS is increased by 50% above the base SS.
          let driverHasMatches = this.drivers.array.map((driver) =>
            this.matchCondition(address.length, driver.length)
          )
          this.driverSuitability = this.driverSuitability.map((suitability, index) => {
            if (driverHasMatches[index]) {
              return (suitability *= 1.5)
            } else {
              return suitability
            }
          })
          this.scores.push({ score: this.driverSuitability, address: address })
        } else {
          // Odd case: SS = the number of consonants in the driver’s name.
          this.driverSuitability = this.drivers.array.map((driver) => this.countConsonants(driver))
          // If matches exists the SS is increased by 50% above the base SS.
          let driverHasMatches = this.drivers.array.map((driver) =>
            this.matchCondition(address.length, driver.length)
          )
          this.driverSuitability = this.driverSuitability.map((suitability, index) => {
            if (driverHasMatches[index]) {
              return (suitability *= 1.5)
            } else {
              return suitability
            }
          })

          this.scores.push({ score: this.driverSuitability, address: address })
        }
      })

      while (this.results.length !== this.drivers.array.length) {
        let scoresMax = this.scores.map((s) => Math.max(...s.score))
        let maxScore = Math.max(...scoresMax)
        let indexScoresMax = scoresMax.indexOf(maxScore)
        let driverIndex = this.scores[indexScoresMax].score.indexOf(maxScore)
        let address = this.scores[indexScoresMax].address
        let driver = this.drivers.array[driverIndex]

        this.results.push({ driver, address, ss: maxScore })
        this.scores.splice(indexScoresMax, 1)
        this.scores.forEach((s) => {
          s.score[driverIndex] = 0
        })
      }

      this.exportToCsv()
    },
  },
}
</script>
