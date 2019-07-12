<template>
    <div>
     <v-layout row justify-center>
    <v-dialog v-model="dialog" fullscreen hide-overlay transition="dialog-bottom-transition">
      <v-btn slot="activator" color="primary" dark>NUEVA TAREA</v-btn>
      <v-card>
        <v-toolbar dark color="primary">
          
          <v-toolbar-title>TAREAS!!</v-toolbar-title>
          <v-spacer></v-spacer>
          <v-toolbar-items>
            <v-btn icon dark @click="dialog = false">
            <v-icon>close</v-icon>
          </v-btn>
          </v-toolbar-items>
        </v-toolbar>

        <v-list three-line subheader>

          <v-subheader>
            AGREGA TUS TAREAS
          </v-subheader>
          
        </v-list>
        <v-divider></v-divider>
        <form>
             
          <v-text-field
            :value="user.usuario"
            label="usuario"
            outline
            readonly
          ></v-text-field>
       
          <v-text-field
            label="tareas realizadas"
            name="tareasSi"
            v-model="user.tareasSi"
            textarea
            autofocus
          ></v-text-field>
         
          <v-text-field    
            label="tareas por realizar"
            name="tareasNo"
            v-model="user.tareasNo"
            textarea
          ></v-text-field>
          
          <v-checkbox    
            value="1"
            label="Option"
            data-vv-name="checkbox"
            type="checkbox"
            required
          ></v-checkbox>

          
          <v-btn color="null">clear</v-btn>
          <v-btn color="success" @click="enviarmensaje">submit</v-btn>
        </form>
      </v-card>
    </v-dialog>
  </v-layout>

      <v-data-table
        :headers="headers"
        :items="users"
        :loading="loading"
        class="elevation-1"
        :no-data-text="$t('admin.usersTable.empty')"
      >
        <template slot="items" slot-scope="props">
          <td class="text-xs-right">{{ props.item.tareano }}</td>
          <td class="text-xs-right">{{ props.item.tareasi }}</td>
          <td class="text-xs-right">{{ props.item.username }}</td>

          <td class="justify-center layout px-0">
            <v-btn icon class="mx-0" @click="editUser(props.item)">
              <v-icon color="teal">edit</v-icon>
            </v-btn>

            <v-btn icon class="mx-0" @click="removeUser(props.item)">
              <v-icon color="pink">delete</v-icon>
            </v-btn>
          </td>

        </template>
      </v-data-table>
    </div>
</template>

<script>
  import * as faker from 'faker';
  import {db} from "@/main";
   import {mapGetters} from 'vuex';
  export default {
		name: "admin-users",
    
    data () {
      return {
        headers: [
          {text: 'tareas Realizadas', value: 'uid', align: 'center'},
          {text: 'tareas Pendientes', value: 'email', align: 'center'},
          {text: this.$t('admin.usersTable.username'), value: 'username', align: 'center'},
          {text: this.$t('common.actions'), value: 'name', sortable: false}
        ],
        users: [],
        loading: true,
        dialog:false,
        /**enviar**/
        user:{
          uid: null,
          tareasSi:null,
          tareasNo:null,
          usuario:  this.$store.state.authModule.user.email
        }
        
      }
    },
    mounted () {
		  this.loading = true;
		  db.collection('props').onSnapshot(snapshot => {
		    this.users = [];
		    snapshot.forEach(user => {
		      const userData = user.data();
		      this.users.push({
            uid: userData.uid,
            tareasi:userData.tareasSi,
            tareano: userData.tareasNo,
            username: userData.usuario || '----'
          });
        });
		    this.loading = false;
      }, (error) => {
        console.log('listener users admin off...');
      })
      
    },
    methods: {
		  editUser (user) {
        this.$store.commit('toggleUsersDialog', {editMode: true, user});
      },
      removeUser (user) {
        console.log(user.uid)
        db.collection('props').doc(user.uid).delete().then(() => {
          this.$store.commit('setAlertMessage', {
            show: true,
            type: 'success',
            message: this.$t('messages.deleted', {item: this.$t('common.user')}),
            timeout: 5000
          });
        });
      },
       enviarmensaje(){
        this.user.uid = faker.random.alphaNumeric(16);
     
        const user = Object.assign({},this.user);
        console.log(user);
        db.collection('props').doc(this.user.uid).set(user).then(() => {
          this.user.tareasSi = "",
          this.user.tareasNo = "",
        this.$store.commit('setAlertMessage', {
            show: true,
            type: 'success',
            message: this.$t('messages.saved', {item: this.$t('common.user')}),
            timeout: 5000
          });
          this.close();
          //this.close();
        })
      },
      close(){
        this.dialog = false;
      }
    },
    computed: {
        ...mapGetters(['userForEdit'])
      },
  }
</script>

<style scoped>

</style>
