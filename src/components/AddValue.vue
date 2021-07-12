<template>
  <form @submit="onSubmit" class="add-form">
    <div class="form-control">
      <label>Neerslag</label>
      <input type="text" v-model="rainfall" name="rainfall" placeholder="Neerslag" />
    </div>
    <div class="form-control">
      <label>Maand</label>
      <input
        type="text"
        v-model="month"
        name="month"
        placeholder="Maand van observatie"
      />
    </div>
    <div class="form-control">
      <label>Jaar</label>
      <input
          type="text"
          v-model="year"
          name="year"
          placeholder="Jaar van observatie"
      />
    </div>

    <input type="submit" value="Opslaan" class="btn btn-block" />
  </form>
</template>

<script>
export default {
  name: "AddValue",
  data() {
    return {
      rainfall: null,
      month: "",
      year: ""
    };
  },
  methods: {
    onSubmit(e) {
      e.preventDefault();
      if (this.rainfall == null) {
        alert("Voeg neerslag toe!");
        return false
      }
      const newValue = {
        rainfall: this.rainfall,
        month: this.month,
        year: this.year,
      };
      this.$emit("add-value", newValue);
      this.rainfall = null;
      this.month = "";
      this.year = "";
    },
  },
};
</script>

<style scoped>
.add-form {
  margin-bottom: 40px;
}
.form-control {
  margin: 20px 0;
}
.form-control label {
  display: block;
}
.form-control input {
  width: 100%;
  height: 40px;
  margin: 5px;
  padding: 3px 7px;
  font-size: 17px;
}
.form-control-check {
  display: flex;
  align-items: center;
  justify-content: space-between;
}
.form-control-check label {
  flex: 1;
}
.form-control-check input {
  flex: 2;
  height: 20px;
}
</style>
