<template>
  <div id="app">
    <div class="content">
      <header>
        <div class="info">
          <h1>Covid and Colleges</h1>
          <h2>A crowdsourced database of how Indian universities are handling the Covid-19 crisis.</h2>
          <button>
            <a href="http://bit.ly/CovidAndColleges">Contribute ↗</a>
          </button>
          <p>Last updated at 12 April, 09:15 PM</p>
        </div>
        <div class="illustration">
          <img src="./assets/header.jpg" alt="Fighting Corona Virus" />
        </div>
      </header>
    </div>
  </div>
</template>

<script>
export default {
  name: "app",
  data() {
    return {
      msg: "Welcome to Your Vue.js App"
    };
  }
};
import "reset-css";
</script>

<style lang="scss">
@media screen and (max-width: 767px) {
  //mobile only
  #app {
    padding: 1em;
    .content {
      padding: 1.5em;
      header {
        flex-direction: column-reverse;
        .info{
          h1{
            font-size: 1.5em;
          }
          h2{
            font-size: 0.875em;
          }
          button{
            font-size: 1em;
          }
        }
        .illustration {
          img {
            margin-bottom: 1.5em;
          }
        }
      }
    }
  }
}
@media screen and (min-width: 769px) and (max-width: 1055px) {
  //tab
  #app {
    padding: 2.5em;
    .content {
      padding: 3em;
      header {
        flex-direction: column-reverse;
        .illustration {
          img {
            margin-bottom: 1.5em;
          }
        }
      }
    }
  }
}
@media screen and (min-width: 1056px) {
  //laptops
  #app {
    padding: 2.5em;
    .content {
      padding: 4em;
      header {
        flex-direction: row;
        align-items: center;
        .info {
          max-width: 40%;
          margin-right: 120px;
          h1{
            font-size: 1.75em;
          }
          h2{
            font-size: 1.5em;
          }
          button{
              font-size: 1em;
          }
        }
      }
    }
  }
}

#app {
  background-color: #f6f6f6;
  .content {
    max-width: 1192px;
    margin: 0 auto;
    background-color: #ffffff;
    box-shadow: 0px 6px 12px rgba(43, 143, 143, 0.1);
    border-radius: 2px;
    header {
      display: flex;
      justify-content: center;
      .info {
        display: flex;
        flex-direction: column;
        h1 {
          font-family: "IBM Plex Sans", sans-serif;
          font-weight: 600;
          line-height: 1.5em;
          color: #6d72c5;
        }
        h2 {
          font-family: "IBM Plex Sans", sans-serif;
          font-weight: 400;
          line-height: 1.5em;
          color: rgba(70, 42, 79, 0.75);
          margin: 0.25em 0 1em 0;
        }
        p {
          font-family: "IBM Plex Sans", sans-serif;
          font-size: 0.875em;
          line-height: 1.125em;
          color: rgba(70, 42, 79, 0.5);
          margin-top: 3em;
        }
        button {
          font-family: "IBM Plex Mono", monospace;
          font-weight: 700;
          padding: 8px 12px;
          background: #7bbf75;
          border: 1px solid #6aa565;
          box-sizing: border-box;
          border-radius: 2px;
          cursor: pointer;
          align-self: flex-start;
          a {
            color: #ffffff;
            text-decoration: none;
          }
        }
      }
      .illustration {
        img {
          width: 60vw;
          max-width: 420px;
        }
      }
    }
  }
}
</style>
