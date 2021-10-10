<script>
  let dt = new Date();
</script>

<main>
  <div class="card">
    <section class="leftside">
      <div class="title">Weather App</div>

      <div class="weather-data" style="background: yellow;">
        <div class="content">
          <b class="city">London</b>
          <p>{dt}</p>
          <b class="temp">08°</b>
          <b>Rainy</b>
        </div>
      </div>

      <div style="background: green;">footer</div>
    </section>

    <section class="rightside">
      <div class="top">
        <div class="searchbar">
          <input type="search" placeholder="tap to search" />
        </div>
        <div>
          <p>Recent Search</p>
          <li>London</li>
          <li>Ghana</li>
          <li>Dubai</li>
          <li>Togo</li>
        </div>
      </div>
      <div class="bottom" />
    </section>
  </div>
</main>

<style>
  .card {
    display: grid;
    grid-template-columns: 65% auto;
    background: white;
    height: 600px;
    width: 1000px;
    border-radius: 20px;
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
    background: red;
    font-family: "Lobster";
    height: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
  }
  .weather-data {
    height: 100%;
    width: 100%;
    display: flex;
    flex-direction: column;
    align-items: center;
    color: white;
    /* justify-content: space-around; */
  }
  .temp {
    font-size: 150px;
  }
  .city {
    font-size: 50px;
  }
  .rightside {
    /* padding: 0 1rem; */
    grid-template-rows: 50% 50%;
    width: 100%;
    display: grid;
    justify-content: center;
  }
  input {
    outline: none;
    border: none;
    border: 2px solid black;
    transition: border 500ms;
  }
  input:focus {
    border: 2px solid red;
  }
  .top {
    height: 100%;
  }
  .searchbar {
    display: flex;
    width: 100%;
    flex-direction: row;
  }
</style>
