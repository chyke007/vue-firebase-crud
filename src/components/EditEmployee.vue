<template>
  <div id="edit-employee">
    <h3>Edit Employee</h3>
    <div class="row">
      <form @submit.prevent="updateEmployee" class="col s12">
        <div class="row">
          <div class="input-field col s12">
            <input type="text" disabled v-model="employee_id" required>
          </div>
        </div>
        <div class="row">
          <div class="input-field col s12">
            <input type="text" v-model="name" required>
          </div>
        </div>
        <div class="row">
          <div class="input-field col s12">
            <input type="text" v-model="dept" required>
          </div>
        </div>
        <div class="row">
          <div class="input-field col s12">
            <input type="text" v-model="position" required>
          </div>
        </div>

        <router-link
          :to="{name:'view-employee',params:{employee_id:employee_id}}"
          class="btn grey"
        >Back</router-link>

        <button type="submit" @submit.prevent="updateEmployee" class="btn green">Submit</button>
      </form>
    </div>
  </div>
</template>
<script>
import db from "./firebaseInit";
export default {
  name: "edit-employee",

  data() {
    return {
      employee_id: null,
      name: null,
      dept: null,
      position: null
    };
  },
  beforeRouteEnter(to, from, next) {
    db.collection("employees")
      .where("employee_id", "==", to.params.employee_id)
      .get()
      .then(querySnapshot => {
        console.log(querySnapshot);
        querySnapshot.forEach(doc => {
          next(vm => {
            (vm.employee_id = doc.data().employee_id),
              (vm.name = doc.data().name),
              (vm.dept = doc.data().dept),
              (vm.position = doc.data().position);
          });
        });
      });
  },
  watch: {
    $route: "fetchData"
  },
  methods: {
    fetchData() {
      db.collection("employees")
        .where("employee_id", "==", this.$router.params.employee_id)
        .get()
        .then(querySnapshot => {
          querySnapshot.forEach(doc => {
            (this.employee_id = doc.data().employee_id),
              (this.name = doc.data().name),
              (this.dept = doc.data().dept),
              (this.position = doc.data().position);
          });
        });
    },
    updateEmployee() {
      db.collection("employees")
        .where("employee_id", "==", this.$route.params.employee_id)
        .get()
        .then(querySnapshot => {
          querySnapshot.forEach(doc => {
            doc.ref
              .update({
                name: this.name,
                dept: this.dept,
                position: this.position
              })
              .then(() => {
                this.$router.push({
                  name: "view-employee",
                  params: {
                    employee_id: this.employee_id
                  }
                });
              });
          });
        });
    }
  }
};
</script>
</script>

<style>
</style>

