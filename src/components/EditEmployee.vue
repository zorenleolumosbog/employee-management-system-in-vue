<template>
  <div id="new-employee">
    <h3>Edit Employee</h3>
    <div class="row">
    <form @submit.prevent="updateEmployee" class="col s12">
      <div class="row">
        <div class="input-field col s12">
          <input type="text" v-model="employee_id" required>
        </div>
      </div>
      <div class="row">
        <div class="input-field col s12">
          <input type="text" v-model="fname" required>
        </div>
      </div>
      <div class="row">
        <div class="input-field col s12">
          <input type="text" v-model="mname" required>
        </div>
      </div>
      <div class="row">
        <div class="input-field col s12">
          <input type="text" v-model="lname" required>
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
      <button type="submit" class="btn">Submit</button>
      <router-link to="/" class="btn grey">Cancel</router-link>
    </form>
  </div>
  </div>
</template>

<script>
  import db from './firebaseInit'
  export default {
    name: "edit-employee",
    data(){
      return {
        employee_id: null,
        fname: null,
        mname: null,
        lname: null,
        dept: null,
        position: null
      }
    },
    beforeRouteEnter(to, from, next){
      db.collection('employees')
        .where('employee_id','==',to.params.employee_id)
        .get()
        .then(querySnapshot => {
          querySnapshot.forEach(doc => {
            next(vm => {
              vm.employee_id = doc.data().employee_id
              vm.fname = doc.data().fname
              vm.mname = doc.data().mname
              vm.lname = doc.data().lname
              vm.dept = doc.data().dept
              vm.position = doc.data().position
            })
          })
        })
    },
    watch: {
      '$route': 'fetchData'
    },
    methods: {
      fetchData(){
        db.collection('employees')
          .where('employee_id','==',this.$route.params.employee_id)
          .get()
          .then(querySnapshot => {
            querySnapshot.forEach(doc => {
              this.employee_id = doc.data().employee_id
              this.fname = doc.data().fname
              this.mname = doc.data().mname
              this.lname = doc.data().lname
              this.dept = doc.data().dept
              this.position = doc.data().position
            })
          })
      },
      updateEmployee(){
        db.collection('employees')
          .where('employee_id','==',this.$route.params.employee_id)
          .get()
          .then(querySnapshot => {
            querySnapshot.forEach(doc => {
              doc.ref.update({
                employee_id: this.employee_id,
                fname: this.fname,
                mname: this.mname,
                lname: this.lname,
                dept: this.dept,
                position: this.position
              }).then(()=>{
                this.$router.push({name:'view-employee', params:{employee_id: this.employee_id}})
              })
            })
          })
      }
    }
  }
</script>
