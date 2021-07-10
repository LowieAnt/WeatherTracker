<template>
  <div class="container">
    <Header
        @toggle-add-value="toggleAddValue()"
        :showAddValue="showAddValue"
        title="Neerslag in Wuustwezel"
    />
    <Values
        @delete-value="deleteValue"
        :values="values"
    />
    <div v-if="showAddValue">
      <AddValue @add-value="addValue" />
    </div>
  </div>
</template>

<script>
import Header from "./components/Header";
import Values from "./components/Values";
import AddValue from "@/components/AddValue";

export default {
  name: "App",
  components: {
    Header,
    Values,
    AddValue,
  },
  data() {
    return {
      values: [],
      showAddValue: false,
    };
  },
  methods: {
    async addValue(value) {
      const res = await fetch("api/values", {
        method: "POST",
        headers: {
          "Content-type": "application/json",
        },
        body: JSON.stringify(value),
      });
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
  },
  async created() {
    this.values = await this.fetchValues();
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
  min-height: 300px;
  border: 1px solid steelblue;
  padding: 30px;
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
</style>
