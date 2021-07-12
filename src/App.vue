<template>
  <div class="container" id="headercontainer">
    <Header
        @toggle-add-value="toggleAddValue()"
        :showAddValue="showAddValue"
        title="Neerslag in Wuustwezel"
    />
    <div v-if="showAddValue">
      <AddValue @add-value="addValue" />
    </div>
  </div>
  <div class="container" id="valuecontainer">
    <ValueList
      @toggle-view-all="toggleViewAll()"
      :showViewall="showViewAll"
    />
    <div v-if="showViewAll">
      <Values
          @delete-value="deleteValue"
          :values="values"
      />
    </div>
  </div>
  <div class="container" id="barchartcontainer">
    <BarChart/>
  </div>
</template>

<script>
import Header from "./components/Header";
import Values from "./components/Values";
import AddValue from "@/components/AddValue";
import ValueList from "@/components/ValueList";
import Chart from "chart.js";
import BarChart from "@/components/BarChart";

export default {
  name: "App",
  components: {
    Header,
    Values,
    AddValue,
    ValueList,
    BarChart
  },
  data() {
    return {
      values: [],
      showAddValue: false,
      showViewAll: false,
      rainfalls: [],
      months: [],
      backgroundColors: [],
      borderColors: []
    };
  },
  methods: {
    toggleViewAll() {
      this.showViewAll = !this.showViewAll;
    },
    async addValue(value) {
      let res;
      const currentMonth = await this.fetchValue(this.values.length).then(value => {
        return value.month
      });
      const currentYear = await this.fetchValue(this.values.length).then(value => {
        return value.year
      })
      const currentRainfall = await this.fetchValue(this.values.length).then(value => {
        return value.rainfall
      })
      if (currentMonth === value.month && currentYear === value.year) {
        value.rainfall = +currentRainfall + +value.rainfall
        res = await fetch("api/values/" + this.values.length, {
          method: "PUT",
          headers: {
            "Content-type": "application/json",
          },
          body: JSON.stringify(value),
        });
      } else {
        res = await fetch("api/values", {
          method: "POST",
          headers: {
            "Content-type": "application/json",
          },
          body: JSON.stringify(value),
        });
      }
      const data = await res.json();
      this.values = [...this.values, data];
    },
    toggleAddValue() {
      this.showAddValue = !this.showAddValue;
    },
    async deleteValue(id) {
      const res = await fetch("api/values/" + id, {
        method: "DELETE",
      });
      if (res.status === 200) {
        this.values = this.values.filter((value) => value.id !== id);
      } else {
        alert("Error deleting value!");
      }
      if (confirm("Are you sure you want to delete this item?")) {
        this.values = this.values.filter((value) => value.id !== id);
      }
    },
    async fetchValues() {
      const res = await fetch("api/values");
      return await res.json();
    },
    async fetchValue(id) {
      const res = await fetch("api/values/" + id);
      return await res.json();
    },
    async makeArrays() {
      await fetch("api/values")
          .then(res => res.json())
          .then(values => {
            Array.from(values).forEach(value => {
              this.months.push(value.month + " " + value.year)
              this.rainfalls.push(value.rainfall)
              switch (value.year) {
                case "2020":
                  this.backgroundColors.push('rgba(54, 162, 235, 0.2)');
                  this.borderColors.push('rgba(54, 162, 235, 1)');
                  break;
                case "2021":
                  this.backgroundColors.push('rgba(255, 206, 86, 0.2)');
                  this.borderColors.push('rgba(255, 206, 86, 1)');
                  break;
                case "2022":
                  this.backgroundColors.push('rgba(153, 102, 255, 0.2)');
                  this.borderColors.push('rgba(153, 102, 255, 1)');
                  break;
                case "2023":
                  this.backgroundColors.push('rgba(255, 99, 132, 0.2)');
                  this.borderColors.push('rgba(255, 99, 132, 1)');
                  break;
                default:
                  this.backgroundColors.push('rgba(255, 206, 86, 0.2)');
                  this.borderColors.push('rgba(255, 206, 86, 1)');
              }
            })
          })
          .then(() => {
            this.makeBarChart()
          })
    },
    async makeBarChart() {
      const ctx = document.getElementById('barChart').getContext('2d');
      new Chart(ctx, {
        type: 'bar',
        data: {
          labels: this.months,
          datasets: [{
            label: 'Hoeveelheid neerslag',
            data: this.rainfalls,
            backgroundColor: this.backgroundColors,
            borderColor: this.borderColors,
            borderWidth: 1
          }]
        },
        options: {
          scales: {
            y: {
              beginAtZero: true
            }
          }
        }
      });
    }
  },
  async created() {
    this.values = await this.fetchValues();
    await this.makeArrays();
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Poppins:wght@300;400&display=swap");
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}
body {
  font-family: "Poppins", sans-serif;
}
.container {
  max-width: 500px;
  margin: 30px auto;
  overflow: auto;
  border: 1px solid steelblue;
  padding: 10px;
  border-radius: 5px;
}
.btn {
  display: inline-block;
  background: #000;
  color: #fff;
  border: none;
  padding: 10px 20px;
  margin: 5px;
  border-radius: 5px;
  cursor: pointer;
  text-decoration: none;
  font-size: 15px;
  font-family: inherit;
}
.btn:focus {
  outline: none;
}
.btn:active {
  transform: scale(0.98);
}
.btn-block {
  display: block;
  width: 100%;
}
#headercontainer {
  margin-top: 0;
  margin-left: 50px;
  float: left;
  padding: 20px;
}
#valuecontainer {
  margin-top: 0;
  margin-right: 50px;
  float: right;
}
#barchartcontainer {
  margin-top: 50px;
}
</style>
