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
        <dynamic-line-chart source="topContributors"
                    title="Top Authors By Commit"
                    cite-url=""
                    cite-text="Authors by Commit">
        </dynamic-line-chart>
      </div>

      <div class="col col-6">
        <dynamic-line-chart source="openness"
                    title="Repo Openness"
                    cite-url=""
                    cite-text="Repo Openness"
                    :data="values['openness']"
                    >
        </dynamic-line-chart>
      </div>
      <div class="col col-12">
        <h3>Days until this Repo is considered inactive:  </h3>
      </div>
    </div>
  </section>
</template>
<script>

import DynamicLineChart from './charts/DynamicLineChart'
import BubbleChart from './charts/BubbleChart'
import StackedBarChart from './charts/StackedBarChart'
import DualAxisContributions from './charts/DualAxisContributions'

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
    DualAxisContributions
  }
}

created () {
      //THIS BLOCK EXECUTES WHENEVER THIS CARD FILE GETS RENDERED,
      // IF YOU WANT TO CALL YOUR ENDPOINT IN YOUR CARD FILE,
      // THIS IS WHERE/HOW YOU SHOULD DO IT:

      let repo = window.AugurAPI.Repo({ githubURL: this.repo })
      repo[this.source]().then((data) => {
        // IF YOU ARE CREATING YOUR OWN CHART FILE, SET THIS.VALUES LIKE THIS
        //this.values['endpoint_name'] = data

        // IF YOU ARE USING THE DYNAMIC LINE CHART, SET IT LIKE THIS
        this.values['openness'][this.repo]['openness']
        // this is so that 'this.values['endpoint_name'] = ' something with the following format (this is how DynamicLineChart.vue wants the data to be formatted):
            /*
            {
            	repo_name: {
            		endpoint_name: {
            			[
            				{data}, {data}, {data}
            			]
            		}
            	}
            }
            */
      })
      //FINISH CALLING ENDPOINT
  }

</script>
