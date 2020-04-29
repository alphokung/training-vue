<template>
  <div id="app" style="padding:20px">
    <h1 class="is-size-4">Please enter your data</h1>
    <div v-if="warningMsg" class="has-text-danger">{{warningMsg}}</div>
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
              <th>Type</th>
              <th>Description</th>
              <th>Fine</th>
              <th>Is Paid</th>
              <th>#</th>
            </tr>
          </thead>
          <tbody>
            <tr v-if="this.results.length ==0">
              <td colspan="5">
                No Data
              </td>
            </tr>
            <tr v-for="result in results" :key="result.Id">
              <td>{{result.type}}</td>
              <td>{{result.dectiption}}</td>
              <td>{{result.fine}}</td>
              <td>{{result.isPaid?"จ่ายแล้ว":"ยังไม่จ่าย"}}</td>
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
      warningMsg: null,
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
  methods: {
    onSubmit() {
      if (
        this.form.type &&
        this.form.type &&
        this.form.dectiption &&
        this.form.fine
      ) {
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
        this.form = {
          Id: null,
          type: "",
          dectiption: "",
          fine: null,
          isPaid: false
        };
        this.isEditId = null;
      } else {
        this.warningMsg = "Please enter all fields before submit.";
      }
    },
    onUpdate(item) {
      this.form.Id = item.Id;
      this.form.type = "HELLO";
      this.form.dectiption = item.dectiption;
      this.form.fine = item.fine;
      this.form.isPaid = item.isPaid;

      this.isEditId = item.Id;
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
