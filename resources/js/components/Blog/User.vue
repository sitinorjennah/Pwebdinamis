<template>
    <div class="container">
        <div class="row justify-content-center">
            <div class="col-md-12">
                <div class="card">
                    <div class="card-header">Data User</div>

                    <div class="card-body">
                        <button type="button" class="btn btn-success float-right" @click="modalBaru"><i class="fas fa-user-friends nav-icon"></i> Tambah User
                        </button>        
                        <table class="table table-bordered">  
                            <thead>
                            <tr>
                            <th style="width :10%"><center>Id </center></th>
                            <th style="width :30%"><center>Nama </center></th> 
                            <th style="width :20%"><center>Email </center></th>
                            <!-- <th style="width :20%"><center>Password </center></th> -->
                            <th style="width :20%"><center>Aksi </center></th>    
                            </tr>
                            </thead>
                    <tbody>
                    <tr v-for="items in users" :key="items.id">
                        <td>{{items.id}}</td>
                        <td>{{items.name}}</td>
                        <td>{{items.email}}</td>     
                        <!-- <td>{{items.password}}</td>   -->
                        <td>
                            <center>
                            <a href="#" @click="editData(items)" title="Edit Data">
                            <i class="fas fa-edit pink"> &nbsp; |  &nbsp; </i>
                            </a>
                            |
                            <a href="#" @click="deleteData(items.id)" title="Hapus Data">
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
        <!-- Modal -->
    <div
      class="modal fade"
      id="tambah"
      tabindex="-1"
      role="dialog"
      aria-labelledby="tambahLabel"
      aria-hidden="true"
    >
      <div class="modal-dialog modal-dialog-centered" role="document">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title" id="tambahLabel" v-show="!editmode">Tambah Data Baru</h5>
            <h5 class="modal-title" id="tambahLabel" v-show="editmode">Rubah Data Kategori</h5>
            <button type="button" class="close" data-dismiss="modal" aria-label="Close">
              <span aria-hidden="true">&times;</span>
            </button>
          </div>
          <form @submit.prevent="editmode ? updateData() : createData()">
            <div class="modal-body">
              <div class="form-group">
                <input
                  v-model="form.name"
                  type="text"
                  name="name"
                  placeholder="Nama User"
                  class="form-control"
                  :class="{ 'is-invalid': form.errors.has('name') }"
                />
                <has-error :form="form" field="name"></has-error>
              </div>

              <div class="form-group">
                <input
                  v-model="form.email"
                  type="text"
                  name="email"
                  placeholder="Email"
                  class="form-control"
                  :class="{ 'is-invalid': form.errors.has('email') }"
                />
                <has-error :form="form" field="email"></has-error>
              </div>

            <div class="form-group">
                <input
                  v-model="form.password"
                  type="text"
                  name="password"
                  placeholder="Password"
                  class="form-control"
                  :class="{ 'is-invalid': form.errors.has('password') }"
                />
                <has-error :form="form" field="password"></has-error>
              </div>  
            </div>

            <div class="modal-footer">
              <button type="button" class="btn btn-danger" data-dismiss="modal">Keluar</button>
              <button v-show="!editmode" type="submit" class="btn btn-primary">Tambah</button>
              <button v-show="editmode" type="submit" class="btn btn-primary">Rubah</button>
            </div>
          </form>
        </div>
      </div>
    </div>
    <!--/Modal-->
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
                    email: "",
                    password: "",
                })
            };
        },
         methods: {
            modalBaru() {
                this.editmode = false; //satu modal dua aksi bisa simpan dan edit
                this.form.reset();
                $("#tambah").modal("show");
            },
            editData(items) {
                this.editmode = true; 
                this.form.reset();
                $("#tambah").modal("show");
                this.form.fill(items);
            },
            
            loadData() { //methods dari semua form CRUD Masuk ke dalam methods 
            axios.get("api/user").then(({ data }) =>(this.users = data)); 
            //untuk menampilkan data
        },
        createData() {
        this.form
            .post("api/user")
            .then(() => {
            this.$Progress.start();
            Fire.$emit("refreshData");
            $("#tambah").modal("hide"); //modal hilang
            // Toast.fire({ //notifikasi di atas
            //     type: "success",
            //     title: "Data Berhasi Tersimpan"
            // });
            this.$Progress.finish();
            })
            .catch();
        },
        updateData(){
          this.form
            .put("api/user/" + this.form.id)
            .then(() => {
              this.$Progress.start();
              $("#tambah").modal("hide");
              Toast.fire({ //notifikasi di atas
                  type: "success",
                  title: "Data Berhasi Terubah"
              });
              this.$Progress.finish();
              Fire.$emit("refreshData");
            })
            .catch(() => {
              this.$Progress.fail();
            });
        },
        deleteData(id) {
          Swal.fire({
            title: "Anda Yakin Ingin Menghapus Data ini?",
            text: "Klik Batal Untuk Membatalkan Penghapusan",
            type: "Warning",
            showCancelButton: true,
            confirmButtonColor: "#3490dc",
            cancelButtonColor: "#d33",
            confirmButtonText: "Hapus"
           })
           .then(result => {
             if (result.value){
              this.form
                .delete("api/user/" + id)
                .then(() => {
                  Swal.fire("Terhapus", "Data Anda Sudah Terhapus", "success");
                  Fire.$emit("refreshData");
                })

                .catch(() => {
                  Swal.fire("Gagal", "Data Anda Gagal Terhapus", "warning");
                });
            }
          });
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
