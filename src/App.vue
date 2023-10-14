<template>

  <div id="appDiv">
    <div>
      <div>
        <input type="text" placeholder="Enter your location . . ." id="locationSearchBar" v-model="location" @keypress="fetchWeather">
      </div>
    
      <div id="weatherContainer">
        <div id="currentBar">
          <span>CURRENT</span>
          <div id="locationPill">
            <span id="locationLbl">Wii Weather</span>
          </div>
        </div>

        <span id="temperatureLbl">°C</span>
        <span id="weatherConditionsLbl">Clear</span>

        <div id="footerBar">
          <span>supported by openweathermap</span>
          <div id="timeBar">
            <span id="timeLbl">As of 00/00/0000</span>
          </div>
        </div>
      </div>

      <!-- only visible if weather.main is not undefined -->
      <!-- <div v-if="(typeof weather.main != 'undefined')" id="weatherWidget">
        <div>{{weather.name}}, {{ weather.sys.country }}</div>
        <div>{{dateBuilder()}}</div>
        <div>{{weather.main.temp}} °C</div>
        <div>{{weather.weather[0].main}}</div>
      </div> -->

    </div>
  </div>

  
</template>

<script>
  export default {
    name: 'app',
    // function called data  
    data () {
      // that returns a JavaScript object
      return {
          api_key: '988974e6af7975818686e94facbfda42',
          url_base: 'http://api.openweathermap.org/data/2.5/',
          location: '',
          weather: {}
      }
    },
    methods: {
      fetchWeather (e) {
        if (e.key == "Enter") {
          console.log("fetchWeather started...")
          // template literals require backticks in JavaScript ( ` ` )
          fetch(`${this.url_base}weather?q=${this.location}&units=metric&APPID=${this.api_key}`)
            .then(res => {
              return res.json();
            }).then(this.setResults).then(this.updateLabels);
        }
      },
      setResults(results){
        this.weather = results;
      },
      updateLabels(){
        console.log("update labels started...")
        document.querySelector("#locationLbl").innerHTML = `${this.weather.name}, ${this.weather.sys.country }`;
        document.querySelector("#temperatureLbl").innerHTML = `${this.weather.main.temp}°C`;
        document.querySelector("#weatherConditionsLbl").innerHTML = `${this.weather.weather[0].main}`;

        let d = new Date().toLocaleDateString();
        document.querySelector('#timeLbl').innerHTML = `As of ${d}`;
      }
    }
  }
</script>

<style scoped>

  /* IDs */
  #appDiv {
    display: flex;
    justify-content: center;
  }

  #weatherContainer {
    width: 500px;
    height: 250px;
    background: linear-gradient(160deg, rgba(10,161,240,1) 0%, rgba(9,18,128,1) 90%);
    overflow: hidden;
    position: relative;
    border-radius: 3px;
    box-shadow: 0px 2px 5px  1px #868686;
  }
  #currentBar {
    background: linear-gradient(90deg, rgba(10,95,12,1) 10%, rgba(12,131,9,1) 20%, rgba(10,95,12,1) 30%);
    position: relative;
    top: 5px;
    font-size: 25px;
    color: white;
    display: flex;
    padding: 3px 0 3px 5px;
    box-shadow: 0px 2px 3px  1px #0D4376;
    text-shadow: 3px 2px 3px #0D4376;
  }
  #locationPill {
    background: linear-gradient(180deg, rgba(9,140,34,1) 0%, rgba(8,186,87,1) 43%);
    border: 0px transparent;
    border-radius: 25px 0 0 25px;
    text-align: end;
    width: 250px;
    margin-left: auto;
    box-shadow: -2px -2px 3px 1px #0D4376;
  }
  #footerBar {
    display: flex;
    color: white;
    font-size: 14px;
    background: linear-gradient(90deg, rgba(39,142,219,0.4) 0%, rgba(45,145,218,1) 18%, rgba(25,96,177,0.4) 71%);
    position: relative;
    top: 180px;
    padding-left: 5px;
  }
  #timeBar {
    background: linear-gradient(90deg, rgba(133,71,94,0) 0%, rgba(181,60,62,1) 50%);
    text-align: end;
    width: 250px;
    margin-left: auto;
    border-bottom: 2px solid white;
    border-image: linear-gradient(90deg, rgba(255, 255, 255, 0), rgb(255, 255, 255));
    border-image-slice: 1;
  }
  #temperatureLbl {
    position: absolute;
    top: 60px;
    left: 90px;
    font-size: 50px;
    font-weight: 600;
    color: white;
    text-shadow: 3px 2px 3px #294D5A;
  }
  #weatherConditionsLbl {
    position: absolute;
    top: 140px;
    left: 100px;
    font-size: 30px;
    font-weight: 300;
    color: white;
    text-shadow: 3px 2px 3px #294D5A;
  }
  #locationLbl {
    padding-right: 10px;
  }


  /* Classes */

</style>
