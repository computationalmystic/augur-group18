<template>
  <section>
    <h1>Group 18 Features</h1>
    <div style="display: inline-block;">
      <h2 style="display: inline-block; color: black !important">{{ $store.state.baseRepo }}</h2>
      <h2 style="display: inline-block;" class="repolisting" v-if="$store.state.comparedRepos.length > 0"> compared to: </h2>
      <h2 style="display: inline-block;" v-for="(repo, index) in $store.state.comparedRepos">
        <span v-bind:style="{ 'color': colors[index] }" class="repolisting"> {{ repo }} </span>
      </h2>
    </div>
    <div class="row">
      <div class="col col-6">
        <top-authors></top-authors>
      </div>

      <div class="col col-6">
        <dynamic-line-chart source="totalCommitters"
                    title="Repo Openness"
                    cite-url=""
                    cite-text="Repo Openness"
                    >
        </dynamic-line-chart>
      </div>
      <div class="col col-12">
        <h3 style="text-align: center" id = "daysLeft">Days until this Repo is considered inactive:  </h3>
      </div>
    </div>
  </section>
</template>
<script>

import DynamicLineChart from './charts/DynamicLineChart'
import BubbleChart from './charts/BubbleChart'
import StackedBarChart from './charts/StackedBarChart'
import DualAxisContributions from './charts/DualAxisContributions'
import TopAuthors from './charts/TopAuthors'

var archiveMonth = Math.floor((Math.random() * 12) + 1);
var archiveDay = Math.floor((Math.random() * 28) + 1);
var archiveDate = new Date(2019, archiveMonth, archiveDay);
module.exports = {
  data() {
    return {
      colors: ["#FF3647", "#4736FF","#3cb44b","#ffe119","#f58231","#911eb4","#42d4f4","#f032e6"]
    }
  },
  components: {
    DynamicLineChart,
    BubbleChart,
    StackedBarChart,
    DualAxisContributions,
    TopAuthors
  }
}
var countDownDate = archiveDate.getTime();

// Update the count down every 1 second
var x = setInterval(function() {

  // Get todays date and time
  var now = new Date().getTime();

  // Find the distance between now and the count down date
  var distance = countDownDate - now;

  // Time calculations for days, hours, minutes and seconds
  var days = Math.floor(distance / (1000 * 60 * 60 * 24));
  var hours = Math.floor((distance % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60));
  var minutes = Math.floor((distance % (1000 * 60 * 60)) / (1000 * 60));
  var seconds = Math.floor((distance % (1000 * 60)) / 1000);

  // Output the result in an element with id="demo"
  document.getElementById("daysLeft").innerHTML = days;

  // If the count down is over, write some text
  if (distance < 0) {
    clearInterval(x);
    document.getElementById("daysLeft").innerHTML = "INACTIVE";
  }
}, 1000);
</script>
