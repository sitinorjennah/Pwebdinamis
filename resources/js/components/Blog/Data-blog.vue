<template>
    <div class="container">
        <div class="row justify-content-center">
            <div class="col-md-12">
                <div class="card">
                    <div class="card-header">Data Blog</div>

                    <div class="card-body">
                        <button type="button" class="btn btn-success float-right"><i class="fas fa-th nav-icon"></i> Tambah Blog
                        </button>        
                        <table class="table table-bordered">  
                            <thead>
                    <tr>
                      <th style="width :15%"><center>Id </center></th>
                      <th style="width :15%"><center>Kategori_id </center></th> 
                      <th style="width :15%"><center>User_id </center></th>
                      <th style="width :15%"><center>Judul </center></th>
                      <th style="width :15%"><center>Isi </center></th>
                      <th style="width :15%"><center>Jbaca </center></th> 
                      <th style="width :35%"><center>Aksi </center></th>    
                    </tr>
                    </thead>
                    <tbody>
                    <tr v-for="items in blogs" :key="items.id">
                        <td>{{items.id}}</td>
                        <td>{{items.kategori_id}}</td>
                        <td>{{items.user_id}}</td>
                        <td>{{items.judul}}</td>
                        <td>{{items.isi}}</td>
                        <td>{{items.jbaca}}</td>

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
                blogs: {}, //aray yang menmpung data dari table
                form: new Form({ //untuk menyimpan data
                    id: "",
                    kategori_id: "",
                    user_id: "",
                    judul: "",
                    isi: "",
                    jbaca: ""
                })
            };
        },
        methods: {
            loadData() { //methods dari semua form CRUD Masuk ke dalam methods 
            axios.get("api/blog").then(({ data }) =>(this.blogs = data)); 
            //untuk menampilkan dat
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
