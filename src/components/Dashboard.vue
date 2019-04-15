<template>
  <div id="dashboard">
    <center v-if="set === false">
      <Preloader class="preload"></Preloader>
    </center>
    <ul class="collection with-header" v-else>
      <li class="collection-header">
        <h4>Employees</h4>
      </li>
      <li v-for="employee in employees" :key="employee.id" class="collection-item">
        <div class="chip">{{employee.dept}}</div>
        {{employee.name}}
        <router-link
          class="secondary-content"
          :to="{name:'view-employee',params:{employee_id:employee.employee_id}}"
        >
          <i class="fa fa-eye"></i>
        </router-link>
      </li>
    </ul>

    <div class="fixed-action-btn">
      <router-link to="/new" class="btn-floating btn-large red">
        <i class="fa fa-plus"></i>
      </router-link>
    </div>
  </div>
</template>
<script>
import db from "./firebaseInit";
import Preloader from "./Preloader";

export default {
  name: "dashboard",
  data() {
    return {
      employees: [],
      set: false
    };
  },
  components: {
    Preloader
  },
  created() {
    db.collection("employees")
      .orderBy("dept")
      .get()
      .then(querySnapshot => {
        querySnapshot.forEach(doc => {
          console.log(doc.data());
          const data = {
            id: doc.id,
            employee_id: doc.data().employee_id,
            name: doc.data().name,
            dept: doc.data().dept,
            position: doc.data().position
          };
          this.employees.push(data);
        });
        this.set = true;
      });
  },
  method: {}
};
</script>

<style>
.preload {
  margin: 10px;
}
</style>

