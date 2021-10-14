<script>
  import Modal from "./Modal.svelte";
  import { onMount } from "svelte";
  let city, temperature, windsped, desc, humid, press, descIcon;
  let input = "";
  let Location;
  let url = `https://source.unsplash.com/1600x900/?${city}`;
  let weather = {
    apikey: "9b746fbe148e6509b9ccf883808039cf",
    GetWeather: function (city) {
      fetch(
        `http://api.openweathermap.org/data/2.5/weather?q=${city}&units=metric&appid=${this.apikey}`
      )
        .then((response) => response.json())
        .then((data) => this.displayWeather(data))
        .catch(function (err) {
          console.log("error!!: " + err.message);
        });
    },

    displayWeather: function (data) {
      let { name } = data;
      const { icon, description } = data.weather[0];
      const { temp, humidity, pressure } = data.main;
      const { speed } = data.wind;
      console.log(name, icon, description);
      city = name;
      temperature = temp + "°C";
      windsped = speed + "km/h";
      desc = description;
      humid = humidity + "%";
      press = pressure + "mb";
      descIcon = "https://openweathermap.org/img/wn/" + icon + "@2x.png";
      url = `https://source.unsplash.com/1600x900/?${city}`;
    },
    Search: function () {
      this.GetWeather(input);
    },
  };

  let showModal = false;

  const openSearch = () => {
    showModal = !showModal;
  };

  const handleSearch = () => {
    console.log(input);
    weather.Search();
    showModal = false;
  };

  onMount(async () => {
    const response = await fetch("https://geolocation-db.com/json/");
    const locationn = await response.json();
    if (locationn.city === null) {
      Location = locationn.country_name;
    } else {
      Location = locationn.city;
    }
    weather.GetWeather(Location);
  });
</script>

<main>
  <div class="card" style="background: url({url});">
    <section class="leftside">
      <h3 class="title">Weather App</h3>
      {#if city === undefined}
        <div class="loading">
          <img src="/imgs/weather.png" alt="" class="pulse" width="100px" />
          <p>Loading...</p>
        </div>
      {:else}
        <div class="weather-data">
          <div class="content">
            <b class="city">{city}</b>

            <b class="temp">{temperature}</b>
            <div class="description">
              <img src={descIcon} alt="" />
              <b>{desc}</b>
            </div>
          </div>
        </div>

        <div class="bottom">
          <div class="weatherdetails">
            <div class="Humidity weather">
              Humidity

              <h4>{humid}</h4>
            </div>
            <div class="Wind weather">
              Wind Speed
              <h4>{windsped}</h4>
            </div>
            <div class="Rain weather">
              Pressure
              <h4>{press}</h4>
            </div>
          </div>
        </div>
      {/if}
    </section>

    <section class="rightside">
      <div class="container">
        <div class="searchbar">
          <input
            type="search"
            placeholder="tap to search"
            on:keyup|preventDefault={handleSearch}
            bind:value={input}
          />
          <img
            src="/imgs/search.png"
            alt=""
            on:click|preventDefault={handleSearch}
          />
        </div>
        <div class="footer">
          <p>
            Developed with ❤️ | by <a
              href="https://github.com/lesronn"
              target="_blank"><strong>Lesron</strong></a
            >.
          </p>
        </div>
      </div>
    </section>
  </div>

  <!-- MOBILE VIEEW -->
  <Modal {showModal} on:click={openSearch}>
    <div class="m-searchbar">
      <input
        type="search"
        class="m-input"
        placeholder="tap to search"
        bind:value={input}
      />
      <img
        src="/imgs/search.png"
        alt=""
        on:click|preventDefault={handleSearch}
      />
    </div>
  </Modal>
  <div class="card1" style="background: url({url});">
    <section id="header">
      <h3>Weather App</h3>
      <div class="m-search" on:click={openSearch}>
        <img src="/imgs/search.png" alt="" />
      </div>
    </section>
    {#if city === undefined}
      <div class="loading">
        <img src="/imgs/weather.png" alt="" class="pulse" width="80px" />
        <p>Loading...</p>
      </div>
    {:else}
      <section class="m-body">
        <div class="m-content">
          <b class="m-city">{city}</b>
          <b class="m-temp">{temperature}</b>
          <div class="m-description">
            <img src={descIcon} alt="" />
            <b>{desc}</b>
          </div>
        </div>
      </section>
      <section class="m-bottom">
        <div class="m-weatherdetails">
          <div class="Humidity m-weather">
            Humidity
            <h4>{humid}</h4>
          </div>
          <div class="Wind m-weather">
            Wind Speed
            <h4>{windsped}</h4>
          </div>
          <div class="Rain m-weather">
            Pressure
            <h4>{press}</h4>
          </div>
        </div>
      </section>

      <section class="m-footer">
        <p>
          Developed with ❤️ | by <a
            href="https://github.com/lesronn"
            target="_blank"><strong>Lesron</strong></a
          >.
        </p>
      </section>
    {/if}
  </div>
</main>

<style>
  .card {
    display: grid;
    grid-template-columns: 65% auto;
    height: 600px;
    width: 1000px;
    border: 5px solid white;
    border-radius: 30px !important;
    background-repeat: no-repeat;
    background-attachment: fixed;
    background-size: cover;
    box-shadow: inset 0 0 0 2000px rgba(0, 0, 0, 0.8);
    z-index: 2;
  }
  .card1 {
    display: none;
  }
  .leftside {
    display: grid;
    grid-template-rows: 7% auto 20%;
  }
  .content {
    height: 100%;
    width: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
  }
  .title {
    font-family: "Lobster";
    height: 100%;
    display: flex;
    align-items: end;
    justify-content: center;
    color: white;
  }
  .weather-data {
    height: 100%;
    width: 100%;
    display: flex;
    flex-direction: column;
    align-items: center;
  }
  .temp {
    font-size: 150px;
    color: white;
  }
  .city {
    font-size: 50px;
    padding-bottom: 1rem;
    margin-top: -2rem;
    color: white;
  }
  .description {
    display: flex;
    flex-direction: row;
    justify-content: center;
    align-items: center;
    margin-bottom: -3rem;
    color: white;
  }
  .rightside {
    display: grid;
  }
  .container {
    padding: 1rem;
    height: 100%;
    display: flex;
    width: 100%;
    justify-content: space-between;
    flex-direction: column;
  }
  input {
    outline: none;
    border: none;
    text-indent: 20px;
    height: 50px;
    background: rgb(233, 233, 233);
    font-family: inherit;
    font-size: 16px;
  }

  .searchbar {
    display: grid;
    width: 100%;
    grid-template-columns: 80% auto;
    grid-column-gap: 10px;
    justify-content: center;
    border-radius: 5px;
    background: rgb(233, 233, 233);
    margin-top: 5rem;
  }
  img {
    cursor: pointer;
  }
  .bottom {
    display: grid;
    height: 100%;
    margin: 0 50px;
    align-items: center;
  }
  .weatherdetails {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    height: 100%;
    color: white;
  }
  .weather {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
  }
  .weather h4 {
    padding-top: 15px;
  }
  .footer {
    border-top: 2px solid rgb(206, 206, 206);
    text-align: center;
  }
  .footer p {
    padding-top: 0.5rem;
    color: white;
  }
  .footer a {
    color: white;
  }

  .loading {
    width: 100%;
    height: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
    color: white;
  }

  .pulse {
    margin: 50px;
    display: block;
    width: 100px;
    border-radius: 50%;

    cursor: pointer;

    animation: pulse 2s infinite;
  }
  @-webkit-keyframes pulse {
    0% {
      -webkit-transform: scale(1, 1);
    }
    50% {
      -webkit-transform: scale(1.2, 1.2);
    }
    100% {
      -webkit-transform: scale(1, 1);
    }
  }

  @keyframes pulse {
    0% {
      transform: scale(1, 1);
    }
    50% {
      transform: scale(1.2, 1.2);
    }
    100% {
      transform: scale(1, 1);
    }
  }

  @media only screen and (max-width: 999px) {
    .card {
      display: none;
    }
    .card1 {
      width: 100vw;
      height: 100vh;
      border-radius: 0px !important;
      display: grid;
      grid-template-rows: 7% 66% 20% 7%;
      overflow: hidden !important;
      background-repeat: no-repeat;
      background-attachment: fixed;
      background-size: cover;
      background-position: center center;
      box-shadow: inset 0 0 0 2000px rgba(0, 0, 0, 0.7);
      z-index: 2;
    }
    #header {
      display: grid;
      text-align: center;
      height: 100%;
      width: 100%;
      grid-template-columns: 85% auto;
      align-items: center;
      border-bottom: 1.5px solid rgb(218, 218, 218);
      background: white;
      font-family: "lobster";
    }
    #header h3 {
      margin-left: 30px;
    }
    #header img {
      width: 2rem;
      margin-right: 3rem;
      cursor: pointer;
    }
    .m-body {
      text-align: center;
      color: white;
    }
    .m-content {
      padding: 10px;
      display: grid;
      grid-template-rows: repeat(3, 1fr);
      height: 100%;
      width: 100%;
      justify-content: center;
    }
    .m-temp {
      font-size: 80px;
    }
    .m-city {
      font-size: 50px;
    }
    .m-description {
      display: flex;
      flex-direction: column;
      height: 100%;
      width: 100%;
      justify-content: start;
      align-items: center;
      text-transform: capitalize;
      font-size: 18px;
    }
    .m-bottom {
      display: grid;
      align-items: center;
      color: white;
      height: 100%;
      width: 100%;
    }

    .m-weather {
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
    }
    .m-weather h4 {
      padding-top: 15px;
    }
    .m-weatherdetails {
      display: grid;
      grid-template-columns: repeat(3, 1fr);
      height: 100%;
    }
    .m-footer {
      display: flex;
      height: 100%;
      align-items: center;
      justify-content: center;
      border-top: 2px solid rgb(206, 206, 206);
      color: white;
    }
    .m-footer a {
      color: white;
    }
    .m-searchbar {
      display: grid;
      width: 100%;
      grid-template-columns: 80% auto;
      grid-column-gap: 10px;
      justify-content: center;
      border-radius: 5px;
      background: rgb(233, 233, 233);
    }
    .m-input {
      outline: none;
      border: none;
      text-indent: 20px;
      background: rgb(233, 233, 233);
      font-family: inherit;
      font-size: 16px;
      height: 50px;
    }
  }
</style>
