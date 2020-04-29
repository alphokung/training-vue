<template>
  <div id="app" style="padding:20px">
    <h2>Please enter your data</h2>
    <form v-on:submit.prevent="onSubmit">
      <div class="field">
        <label class="label">Type</label>
        <div class="control">
          <input v-model="form.type" class="input" type="text" />
        </div>
      </div>

      <div class="field">
        <label class="label">Description</label>
        <div class="control">
          <input v-model="form.dectiption" class="input" type="text" />
        </div>
      </div>

      <div class="field">
        <label class="label">Fine</label>
        <div class="control">
          <input v-model="form.fine" class="input" type="text" />
        </div>
      </div>

      <div>
        <button class="button" type="submit">Submit</button>
      </div>
      <hr />
      <div>
        <table class="table is-bordered is-striped is-narrow is-hoverable is-fullwidth">
          <thead>
            <tr>
              <td>Type</td>
              <td>Description</td>
              <td>Fine</td>
              <td>Is Paid</td>
              <td>#</td>
            </tr>
          </thead>
          <tbody>
            <tr v-for="result in results" :key="result.Id">
              <td>{{result.type}}</td>
              <td>{{result.dectiption}}</td>
              <td>{{result.fine}}</td>
              <td>{{paidStatus}}</td>
              <td>
                <button
                  type="button"
                  class="button is-small is-primary"
                  v-on:click="onUpdate(result)"
                >Update</button>

                <button
                  type="button"
                  class="button is-small is-success"
                  v-on:click="onPaid(result.Id)"
                >Paid</button>

                <button
                  type="button"
                  class="button is-small is-danger"
                  v-on:click="onDelete(result.Id)"
                >delete</button>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </form>
  </div>
</template>

<script>
export default {
  name: "App",
  data: function() {
    return {
      lastId: 0,
      isEditId: null,
      form: {
        Id: null,
        type: "",
        dectiption: "",
        fine: null,
        isPaid: false
      },
      results: []
    };
  },
  computed: {
    // a computed getter
    paidStatus: function() {
      if (this.isPaid) {
        return "จ่ายแล้ว";
      } else {
        return "ยังไม่จ่าย";
      }
    }
  },
  methods: {
    onSubmit() {
      if (this.isEditId) {
        this.onDelete(this.isEditId);
        let obj = Object.assign({}, this.form);
        obj.Id = this.isEditId;
        this.results.push(obj);
      } else {
        this.lastId++;

        let obj = Object.assign({}, this.form);
        obj.Id = this.lastId;
        this.results.push(obj);
      }
      this.form = {};
      this.isEditId = null;
    },
    onUpdate(item) {
      this.form.Id = item.Id;
      this.form.type = item.type;
      this.form.dectiption = item.dectiption;
      this.form.fine = item.fine;
      this.form.isPaid = item.isPaid;

      this.isEditId = item.Id;
      alert("55");
    },
    onPaid(id) {
      this.results = this.results.map(f =>
        f.Id === id ? { ...f, isPaid: true } : f
      );

      console.log(this.results);
    },
    onDelete(id) {
      this.results = this.results.filter(f => {
        return f.Id !== id;
      });
    }
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}

button {
  margin-left: 10px;
}
</style>
