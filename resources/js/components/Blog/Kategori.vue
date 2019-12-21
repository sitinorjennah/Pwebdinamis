<template>
    <div class="container">
        <div class="row justify-content-center">
            <div class="col-md-12">
                <div class="card">
                    <div class="card-header">Data Kategori</div>

                    <div class="card-body">
                        <button type="button" class="btn btn-success float-right"><i class="fas fa-bars nav-icon"></i>Tambah Blog
                        </button>        
                        <table class="table table-bordered">  
                            <thead>
                    <tr>
                      <th><center>Kategori</center></th>
                      <th style="width :35%"><center>Jumlah Data </center></th> 
                      <th style="width :35%"><center>Aksi </center></th> 
                    </tr>
                    </thead>
                    <tbody>
                    <tr v-for="items in kategoris" :key="items.id">
                        <td>{{items.namakategori}}</td>      
                        <td><center></center></td>
                        <center>
                        <a href="#">
                        <i class="fas fa-edit pink"> | </i>
                        </a>
                        <a href="#">
                        <i class="fas fa-trash teal"></i>
                        </a>
                        </center>
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
                kategoris: {}, //aray yang menmpung data dari table
                form: new Form({ //untuk menyimpan data
                    id: "",
                    namakategori: "",
                    jumlah: ""
                })
            };
        },
        methods: {
            loadData() { //methods dari semua form CRUD Masuk ke dalam methods 
            axios.get("api/kategori").then(({ data }) =>(this.kategoris = data)); 
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
