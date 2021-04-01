<template>
  <div>

    

      <alert-dismiss v-if="$page.props.flash.message" :message="$page.props.flash.message" count="5"></alert-dismiss>


      <button class="btn btn-md btn-info mb-2" @click="create">Nuevo</button>
      <table class="table table-bordered table-sm">
        <thead>
          <th>Nombre</th>
          <th>Email</th>
          <th></th>
        </thead>
        <tbody>
          <tr v-for="user in users.data" :key="user.id">
            <td>{{ user.name }}</td>
            <td>{{ user.email }}</td>
            <td>
              <button class="btn btn-sm btn-warning" @click="edit(user)">Editar</button>
              <button class="btn btn-sm btn-danger" @click="deleteRow(user)">Eliminar</button>
            </td>
          </tr>
        </tbody>
      </table>
      
      <paginations :links="users.links"></paginations>

      <b-modal ref="modal-form" :title="!editMode?'Nuevo Usuario':'Editar Usuario'" hide-footer>
        <form>
          <div class="row">
            <div class="col-md-12 form-group">
              <label class="label-control">Nombre</label>
              <input type="text" v-model="form.name" class="form-control" :class="{'is-invalid':errors.name}">
              <div class="text-danger" v-if="errors.name">{{ errors.name }}</div>
            </div>
          </div>
          <div class="row">
            <div class="col-md-12 form-group">
              <label class="label-control">Email</label>
              <input type="email" v-model="form.email" class="form-control" :class="{'is-invalid':errors.email}">
              <div  class="text-danger"  v-if="errors.email">{{ errors.email }}</div>
            </div>
          </div>
          <div class="row">
            <div class="col-md-12 form-group">
              <label class="label-control">Password</label>
              <input type="password" v-model="form.password" class="form-control" :class="{'is-invalid':errors.password}">
              <div  class="text-danger"  v-if="errors.password">{{ errors.password }}</div>
            </div>
          </div>
          <div class="row">
            <div class="col-md-12 form-group">
              <button v-if="!editMode" type="button" class="btn btn-success" @click="save">Guardar</button>
              <button v-if="editMode" type="button" class="btn btn-success" @click="update(form)">Actualizar</button>
            </div>
          </div>
        </form>
      </b-modal>
  </div>
</template>

<script>
import AppLayout from '@/Layouts/AppLayout'
import Paginations from '@/Shared/Paginations'
import AlertDismiss from '@/Shared/AlertDismiss'
export default {
  components:{
    AppLayout,
    Paginations,
    AlertDismiss
  },
  layout: AppLayout,
  props:['users','errors','flash'],
  data() {
    return {
      editMode: false,
      isOpen: false,
      form: {
        name: null,
        email:null,
        password:null
      },
      dismissCountDown: null,
      showDismissibleAlert: false
      
    }
  },
  methods: {
    create(){
      this.reset();
      this.editMode = false;
      this.$refs['modal-form'].show();
    },
    reset() {
      this.form = {
        name: null,
        email:null,
        password:null
      }
    },
    save: function (event) {
      event.preventDefault();
      this.$inertia.post('/users', this.form)
    },
    edit(data){
      this.form = Object.assign({}, data);
      this.editMode = true;
      this.$refs['modal-form'].show();
    },
    update(data) {
      data._method = 'PUT';
      this.$inertia.visit('/users/' + data.id, {
        method: 'POST',
        data: data,
        onProgress: progress => {
          console.log('holaaaa')
        },
        onSuccess: page => {
          //console.log(page)
        },
      })
      
    },
    deleteRow(data){
      if (!confirm('¿Esta seguro de eliminar?')) return;
      data._method = 'DELETE';
      this.$inertia.post('/users/' + data.id, data);
    },

  },
}
</script>