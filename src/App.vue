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
          <p>{{updateMsg}}</p>
        </div>
        <div class="illustration">
          <img src="./assets/header.jpg" alt="Fighting Corona Virus" />
        </div>
      </header>
      <div class="collegeList">
        <div class="spinner" v-if="loading">
          <div class="dot1"></div>
          <div class="dot2"></div>
        </div>
        <div class="collegeObj" v-else v-for="college in collegeData" :key="college.college_name">
          <h3>{{college.college_name}}</h3>
          <div class="collegeDetails">
            <div class="row r1">
              <div class="section">
                <h4>Lectures</h4>
                <p>{{college.online_class}}</p>
              </div>
              <div class="section">
                <h4>Assignments & tests</h4>
                <p>{{college.assignments}}</p>
              </div>
            </div>
            <div class="row">
              <div class="section">
                <h4>Semester Exams</h4>
                <p>{{college.exam}}</p>
              </div>
              <div class="section">
                <h4>Summer Internships</h4>
                <p>{{college.internship}}</p>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import "reset-css";
import axios from "axios";
export default {
  name: "app",
  data() {
    return {
      msg: "Welcome to Your Vue.js App",
      collegeData: [],
      lastUpdated: "",
      loading: true,
      error: false
    };
  },
  mounted: function() {
    this.makeRequest();
  },
  computed: {
    updateMsg: function() {
      return "Last updated at " + this.lastUpdated;
    }
  },
  methods: {
    makeRequest: function() {
      let vm = this;
      axios
        .get("https://covid-and-colleges.jigar.repl.co/getData")
        .then(function(response) {
          vm.loading = false;
          vm.lastUpdated = response.data.last_entry_added_time;
          vm.collegeData = response.data.data;
          console.log(vm.collegeData);
        })
        .catch(function(error) {
          // handle error
          vm.error = true;
          console.log(error);
        });
    }
  }
};
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
        .info {
          h1 {
            font-size: 1.5em;
          }
          h2 {
            font-size: 0.875em;
          }
          button {
            font-size: 1em;
          }
          p {
            margin-top: 1.5em;
          }
        }
        .illustration {
          img {
            margin-bottom: 1.5em;
          }
        }
      }
    }
    .collegeList {
      grid-gap: 1rem;
      margin-top: 2em;
      grid-template-columns: repeat(auto-fit, minmax(400px, auto));
      .collegeObj {
        padding: 1.5em;
        h3 {
          font-size: 1.25em;
          line-height: 1.3em;
        }
        .collegeDetails {
          flex-direction: column;
          .section {
            margin-top: 1em;
            h4 {
              margin-bottom: 0.25em;
            }
            p {
              padding: 0.25em 0.75em;
              margin-top: 0.5em;
              font-size: 0.875em;
              line-height: 1.5em;
            }
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
    .collegeList {
      grid-gap: 2rem;
      .collegeObj {
        padding: 1.5em;
        h3 {
          font-size: 1.25em;
        }
        .collegeDetails {
          .section {
            h4 {
              margin-bottom: 0.5em;
            }
            p {
            }
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
          h1 {
            font-size: 1.75em;
          }
          h2 {
            font-size: 1.5em;
          }
          button {
            font-size: 1em;
          }
          p {
            margin-top: 3em;
          }
        }
      }
      .collegeList {
        grid-gap: 2rem;
        margin-top: 4em;
        grid-template-columns: repeat(auto-fit, minmax(400px, auto));
        .collegeObj {
          padding: 1.5em;
          h3 {
            font-size: 1.25em;
            line-height: 1.5em;
          }
          .collegeDetails {
            flex-direction: row;
            .row {
              display: flex;
              flex-direction: column;
            }
            .r1 {
              margin-right: 3em;
            }
            .section {
              margin-top: 1.75em;
              h4 {
                margin-bottom: 0.5em;
                font-size: 0.875em;
              }
              p {
                padding: 0.25em 0.75em;
                margin-top: 0.5em;
                font-size: 0.875em;
                line-height: 1.5em;
              }
            }
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
    .collegeList {
      display: grid;
      justify-content: center;
      .collegeObj {
        background: #ffffff;
        border-radius: 4px;
        border: 1px solid rgba(285, 31, 24, 0.05);
        display: flex;
        flex-direction: column;
        justify-content: space-around;
        h3 {
          font-family: "IBM Plex Sans", sans-serif;
          color: #6d72c5;
          font-weight: 600;
        }
        .collegeDetails {
          display: flex;
          .section {
            h4 {
              font-family: "IBM Plex Sans", sans-serif;
              font-style: normal;
              font-weight: 600;
              letter-spacing: 0.05em;
              text-transform: uppercase;
              color: rgba(70, 42, 78, 0.75);
            }
            p {
              border-radius: 2px;
              font-family: "IBM Plex Mono", monospace;
              font-style: normal;
              font-weight: 700;
              font-size: 14px;
              line-height: 18px;
            }
          }
        }
      }

      .good {
        background: rgba(123, 191, 117, 0.1);
        color: #72bb6c;
      }
      .bad {
        background: rgba(235, 62, 111, 0.15);
        color: #eb3e6f;
      }
      .noupdate {
        background: rgba(155, 155, 155, 0.1);
        color: #777;
      }
      .neutral {
        background: rgba(121, 52, 74, 0.1);
        color: #79344a;
      }

      // loader
      .spinner {
        margin: 100px auto;
        width: 40px;
        height: 40px;
        position: relative;
        text-align: center;

        -webkit-animation: sk-rotate 2s infinite linear;
        animation: sk-rotate 2s infinite linear;
      }

      .dot1,
      .dot2 {
        width: 60%;
        height: 60%;
        display: inline-block;
        position: absolute;
        top: 0;
        background-color: #6D72C5;
        border-radius: 100%;

        -webkit-animation: sk-bounce 2s infinite ease-in-out;
        animation: sk-bounce 2s infinite ease-in-out;
      }

      .dot2 {
        top: auto;
        bottom: 0;
        -webkit-animation-delay: -1s;
        animation-delay: -1s;
      }

      @-webkit-keyframes sk-rotate {
        100% {
          -webkit-transform: rotate(360deg);
        }
      }
      @keyframes sk-rotate {
        100% {
          transform: rotate(360deg);
          -webkit-transform: rotate(360deg);
        }
      }

      @-webkit-keyframes sk-bounce {
        0%,
        100% {
          -webkit-transform: scale(0);
        }
        50% {
          -webkit-transform: scale(1);
        }
      }

      @keyframes sk-bounce {
        0%,
        100% {
          transform: scale(0);
          -webkit-transform: scale(0);
        }
        50% {
          transform: scale(1);
          -webkit-transform: scale(1);
        }
      }
    }
  }
}
</style>
