<template>
  <div>
    <highcharts :options="lineChart"></highcharts>
    <highcharts :options="sunburstChart"></highcharts>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import { Chart } from "highcharts-vue";
import Highcharts from "highcharts";
import { sunburstData } from "@/data/sunburst";
import exportingInit from "highcharts/modules/exporting";
import loadSunburst from "highcharts/modules/sunburst.js";
exportingInit(Highcharts);
loadSunburst(Highcharts);

export default defineComponent({
  components: {
    highcharts: Chart,
  },

  setup() {
    const lineChart: Highcharts.Options = {
      title: {
        text: "Highcharts",
      },
      xAxis: {
        categories: ["Jan", "Feb", "Mar"],
      },
      series: [
        {
          name: "A line",
          type: "line",
          data: [1, 3, 2],
          color: "teal",
        },
        {
          name: "B line",
          type: "line",
          data: [2, 1, 1],
          color: "blue",
        },
      ],
      tooltip: {
        formatter: function () {
          return `Some <b>random</b> html`;
        },
      },
    };

    let colors = ["transparent"];
    const defaultColors = Highcharts.getOptions().colors;
    if (defaultColors) {
      colors = ["transparent"].concat(defaultColors);
    }

    const sunburstChart = {
      colors,

      title: {
        text: "World population 2017",
      },

      subtitle: {
        text: 'Source <a href="https://en.wikipedia.org/wiki/List_of_countries_by_population_(United_Nations)">Wikipedia</a>',
      },

      series: [
        {
          type: "sunburst",
          data: sunburstData,
          allowDrillToNode: true,
          cursor: "pointer",
          dataLabels: {
            format: "{point.name}",
            filter: {
              property: "innerArcLength",
              operator: ">",
              value: 16,
            },
            rotationMode: "circular",
          },
          levels: [
            {
              level: 1,
              levelIsConstant: false,
              dataLabels: {
                filter: {
                  property: "outerArcLength",
                  operator: ">",
                  value: 64,
                },
              },
            },
            {
              level: 2,
              colorByPoint: true,
            },
            {
              level: 3,
              colorVariation: {
                key: "brightness",
                to: -0.5,
              },
            },
            {
              level: 4,
              colorVariation: {
                key: "brightness",
                to: 0.5,
              },
            },
          ],
        },
      ],

      tooltip: {
        headerFormat: "",
        pointFormat:
          "The population of <b>{point.name}</b> is <b>{point.value}</b>",
      },
    };

    return {
      lineChart,
      sunburstChart,
    };
  },
});
</script>
