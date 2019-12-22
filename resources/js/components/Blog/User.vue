<template>
    <div class="container">
        <div class="row justify-content-center">
            <div class="col-md-12">
                <div class="card">
                    <div class="card-header">Data User</div>

                    <div class="card-body">
                        <button type="button" class="btn btn-success float-right"><i class="fas fa-user-friends nav-icon"></i> Tambah User
                        </button>        
                        <table class="table table-bordered">  
                            <thead>
                            <tr>
                            <th style="width :25%"><center>Id </center></th>
                            <th style="width :35%"><center>Nama </center></th> 
                            <th style="width :25%"><center>Email </center></th>
                            <th style="width :25%"><center>Aksi </center></th>    
                            </tr>
                            </thead>
                    <tbody>
                    <tr v-for="items in users" :key="items.id">
                        <td>{{items.id}}</td>
                        <td>{{items.name}}</td>
                        <td>{{items.email}}</td>      
                        <td>
                          <center>
                            <a href="#">
                            <i class="fas fa-edit pink">  &nbsp; |  &nbsp; </i>
                            </a>
                            <a href="#">
                            <i class="fas fa-trash teal"></i>
                            </a>
                            </center>
                        </td>
                    </tr>
                    </tbody>
                        </table>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                editmode: false,
                users: {}, //aray yang menmpung data dari table
                form: new Form({ //untuk menyimpan data
                    id: "",
                    name: "",
                    email: ""
                })
            };
        },
        methods: {
            loadData() { //methods dari semua form CRUD Masuk ke dalam methods 
            axios.get("api/user").then(({ data }) =>(this.users = data)); 
            //untuk menampilkan data
        }
        },
        created(){ //untuk menampilkan data
            this.loadData(); //this.form.loadData
            Fire.$on("refreshData", () => {
                this.loadData();
            });
        }
    };
</script>
