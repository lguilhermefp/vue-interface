<template>
  <div id="main-app" class="container">
    <div class="row justify-content-center">
      <add-appointment />
      <appointment-list :appointments="appointments" @remove="removeItem" @edit="editItem" />
    </div>
  </div>
</template>

<script>
  import AppointmentList from './components/AppointmentList';
  import AddAppointment from './components/AddAppointment';
  import _ from 'lodash';
  import axios from 'axios';
  export default {
    name: 'MainApp',
    data: function(){
      return {
        appointments: [],
        aptIndex: 0
      };
    },
    components: {
      AppointmentList,
      AddAppointment
    },
    mounted(){
      axios
        .get('./data/appointments.json')
        .then(response => (this.appointments = response.data.map(item => {
          item.aptId = this.aptIndex;
          this.aptIndex++;
          return item;
        })))
        .catch(e => window.alert(e));
    },
    methods: {
      removeItem: function(apt){
        this.appointments = _.without(this.appointments, apt);
      },
      editItem: function(id, field, text){
        const aptIndex = _.findIndex(this.appointments, {
          aptId: id
        });
        this.appointments[aptIndex][field] = text;
      }
    }
  };
</script>
