<template>
  <div id="app">
    <nav id="nav">
      <img src="./assets/logo.svg" alt="Covid and Colleges" />
      <button>
        <a href="https://bit.ly/gocoronanocorona" target="_blank">Contribute ↗</a>
      </button>
    </nav>
    <div class="container">
      <div class="content">
        <header>
          <div class="info">
            <h1>Covid and Colleges</h1>
            <h2>A crowdsourced database of how Indian universities are handling the Covid-19 crisis.</h2>
            <button>
              <a href="https://bit.ly/gocoronanocorona" target="_blank">Contribute ↗</a>
            </button>
            <p v-if="loading"></p>
            <p v-if="!loading">{{updateMsg}}</p>
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
            <div class="top">
              <h3>{{college.college_name}}</h3>
              <p :class="getStatus(college.college_status)">{{college.college_status}}</p>
              <div class="divider"></div>
            </div>
            <div class="collegeDetails">
              <div class="row r1">
                <div class="section">
                  <h4>Lectures</h4>
                  <p :class="getClass(college.online_class)">{{college.online_class}}</p>
                </div>
                <div class="section">
                  <h4 id="test">Tests & Assignments</h4>
                  <p :class="getClass(college.assignments)">{{college.assignments}}</p>
                </div>
              </div>
              <div class="row">
                <div class="section">
                  <h4>Semester Exams</h4>
                  <p :class="getClass(college.exam)">{{college.exam}}</p>
                </div>
                <div class="section">
                  <h4>Summer Internships</h4>
                  <p :class="getClass(college.internship)">{{college.internship}}</p>
                </div>
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
window.document.body.onscroll = function() {
  if (
    document.body.scrollTop > 240 ||
    document.documentElement.scrollTop > 240
  ) {
    document.getElementById("nav").style.top = "0";
  } else {
    document.getElementById("nav").style.top = "-67px";
  }
};
export default {
  name: "app",
  data() {
    return {
      collegeData: [],
      lastUpdated: "",
      loading: true
    };
  },
  mounted: function() {
    this.makeRequest();
  },
  computed: {
    updateMsg: function() {
      let dateObj = new Date(this.lastUpdated);
      let time = dateObj.toLocaleString("en-US", {
        hour: "numeric",
        minute: "numeric",
        hour12: true
      });
      let date =
        (dateObj + "").split(" ")[1] + " " + (dateObj + "").split(" ")[2];
      return "Last updated at " + date + ", " + time;
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
        })
        .catch(function(error) {
          // handle error
          console.log(error);
        });
    },
    getClass: function(text) {
      let className;
      if (text === ("Digital" || "No Change")) {
        className = "good";
      } else if (text === ("On Hold" || "Cancelled")) {
        className = "bad";
      } else if (text === "No Update") {
        className = "noupdate";
      } else {
        className = "neutral";
      }
      return className;
    },
    getStatus: function(status) {
      let className;
      if (status === "Closed") {
        className = "closed";
      } else {
        className = "opened";
      }
      return className;
    }
  }
};
</script>

<style lang="scss">
@media screen and (max-width: 767px) {
  //mobile only
  nav {
    padding: 1em;
    img {
      height: 16px;
    }
    button {
      margin-right: 2.5em;
      font-size: 12px;
      padding: 6px 10px !important;
    }
  }
  .container {
    padding: 0.75em;
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
            font-size: 0.75em;
          }
          p {
            font-size: 0.875em;
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
      grid-gap: 1.5rem;
      margin-top: 2.5em;
      padding: 0 !important;
      grid-template-columns: repeat(auto-fit, minmax(auto, 400px));
      .collegeObj {
        padding: 1em;
        h3 {
          font-size: 1.25em;
          line-height: 1.3em;
        }
        .collegeDetails {
          flex-direction: row;
          justify-content: space-between;
          .section:first-child {
            margin-bottom: 2em;
            margin-top: 1em;
          }
          .section {
            #test {
              visibility: hidden;
              position: relative;
            }
            #test:after {
              visibility: visible;
              position: absolute;
              top: 0;
              left: 0;
              content: "Assessments";
            }
            h4 {
              margin-bottom: 0.25em;
              font-size: 0.75em;
            }
            p {
              padding: 0.5em 1em;
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
@media screen and (min-width: 769px) and (max-width: 1274px) {
  nav {
    padding: 1em 4em;
    button {
      margin-right: 12em;
      font-size: 12px;
    }
  }
  .container {
    padding: 1.5em;
    .content {
      padding: 3em;
      header {
        flex-direction: row;
        align-items: center;
        .info {
          max-width: 40%;
          margin-right: 120px;
          h1 {
            font-size: 1.5em;
          }
          h2 {
            font-size: 1.2em;
          }
          button {
            font-size: 0.875em;
          }
          p {
            margin-top: 2em;
          }
        }
      }
      .collegeList {
        grid-gap: 3rem;
        margin-top: 5em;
        grid-template-columns: repeat(auto-fit, minmax(400px, auto));
        justify-content: center;
        .collegeObj {
          padding: 1em 2em;
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
              margin-right: 4em;
            }
            .section:first-child {
              margin: 0.5em 0 2em 0;
            }
            .section {
              h4 {
                margin-bottom: 0.5em;
                font-size: 0.75em;
              }
              p {
                padding: 0.5em 1em;
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

@media screen and (min-width: 1275px) {
  nav {
    padding: 1em 8em;
    button {
      margin-right: 24em;
      font-size: 12px;
    }
  }
  .container {
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
        grid-gap: 3rem;
        margin-top: 5em;
        grid-template-columns: repeat(auto-fit, minmax(400px, auto));
        padding: 0 4em !important;
        justify-content: center;
        .collegeObj {
          padding: 1.5em 3em;
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
              margin-right: 4em;
            }
            .section:first-child {
              margin: 0.5em 0 2em 0;
            }
            .section {
              h4 {
                margin-bottom: 0.5em;
                font-size: 0.75em;
              }
              p {
                padding: 0.5em 1em;
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
nav {
  display: flex;
  flex-direction: row;
  align-items: center;
  background: #ffffff;
  box-shadow: 0px 4px 8px rgba(68, 68, 68, 0.05);
  position: fixed;
  justify-content: space-between;
  width: 100%;
  z-index: 999;
  top: -67px;
  transition-duration: 0.3s;
  transition-timing-function: ease-in-out;
  transition-property: all;
  img {
    height: 24px;
  }
}
header,
nav {
  button {
    font-family: "IBM Plex Mono", monospace;
    font-weight: 600;
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
.container {
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
      padding: 0;
      .collegeObj {
        border-radius: 4px;
        display: flex;
        flex-direction: column;
        justify-content: space-around;
        background: #ffffff;
        border: 1px solid #eeeeee;
        box-sizing: border-box;
        //box-shadow: 0px 6px 12px rgba(51, 51, 51, 0.05);
        .top {
          display: flex;
          flex-direction: column;
          h3 {
            font-family: "IBM Plex Sans", sans-serif;
            color: #462a4f;
            font-weight: 600;
          }
          p {
            font-family: "IBM Plex Mono", monospace;
            font-size: 13px;
            line-height: 18px;
            margin-top: 4px;
            font-style: normal;
            font-weight: normal;
          }
          .divider {
            height: 0;
            width: 100%;
            border: 0.5px solid #eee;
            margin: 1em 0;
          }
        }
        .collegeDetails {
          display: flex;
          .section {
            display: flex;
            flex-direction: column;
            align-items: flex-start;
            h4 {
              font-family: "IBM Plex Sans", sans-serif;
              font-style: normal;
              letter-spacing: 0.05em;
              text-transform: uppercase;
              color: #666666;
            }
            p {
              border-radius: 2px;
              font-family: "IBM Plex Mono", monospace;
              font-style: normal;
              font-weight: 600;
            }
          }
        }
      }
      .good {
        background: #f1f9f1;
        color: #6aa565;
      }
      .bad {
        background: #ffedf1;
        color: #d53973;
      }
      .noupdate {
        background: #eee;
        color: #666;
      }
      .neutral {
        background: #fcf8ee;
        color: #d2a226;
      }
      .closed {
        color: #fd436d;
      }
      .opened {
        color: #6d72c5;
      }

      .fade-enter-active,
      .fade-leave-active {
        transition: opacity 0.5s;
      }
      .fade-enter, .fade-leave-to /* .fade-leave-active below version 2.1.8 */ {
        opacity: 0;
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
        background-color: #6d72c5;
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
