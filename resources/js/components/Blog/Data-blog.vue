<template>
    <div class="container">
        <div class="row justify-content-center">
            <div class="col-md-12">
                <div class="card">
                    <div class="card-header">Data Blog</div>

                    <div class="card-body">
                        <button type="button" class="btn btn-success float-right" @click="modalBaru"><i class="fas fa-th nav-icon"></i> Tambah Blog
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
                  v-model="form.kategori_id"
                  type="text"
                  name="kategori_id"
                  placeholder="Kategori Id"
                  class="form-control"
                  :class="{ 'is-invalid': form.errors.has('kategori_id') }"
                />
                <has-error :form="form" field="kategori_id"></has-error>
              </div>

              <div class="form-group">
                <input
                  v-model="form.user_id"
                  type="text"
                  name="user_id"
                  placeholder="User Id"
                  class="form-control"
                  :class="{ 'is-invalid': form.errors.has('user_id') }"
                />
                <has-error :form="form" field="user_id"></has-error>
              </div>

              <div class="form-group">
                <input
                  v-model="form.judul"
                  type="text"
                  name="judul"
                  placeholder="judul"
                  class="form-control"
                  :class="{ 'is-invalid': form.errors.has('judul') }"
                />
                <has-error :form="form" field="judul"></has-error>
              </div>

              <div class="form-group">
                <input
                  v-model="form.isi"
                  type="text"
                  name="isi"
                  placeholder="isi"
                  class="form-control"
                  :class="{ 'is-invalid': form.errors.has('isi') }"
                />
                <has-error :form="form" field="isi"></has-error>
              </div>

              <div class="form-group">
                <input
                  v-model="form.jbaca"
                  type="text"
                  name="jbaca"
                  placeholder="jbaca"
                  class="form-control"
                  :class="{ 'is-invalid': form.errors.has('jbaca') }"
                />
                <has-error :form="form" field="jbaca"></has-error>
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
            axios.get("api/blog").then(({ data }) =>(this.blogs = data)); 
            //untuk menampilkan data
        },
        updateData(){
          this.form
            .put("api/blog/" + this.form.id)
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
                .delete("api/blog/" + id)
                .then(() => {
                  Swal.fire("Terhapus", "Data Anda Sudah Terhapus", "success");
                  Fire.$emit("refreshData");
                })

                .catch(() => {
                  Swal.fire("Gagal", "Data Anda Gagal Terhapus", "warning");
                });
            }
          });
        },
        createData() {
        this.form
            .post("api/blog")
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
        },
            
        created(){ //untuk menampilkan data
            this.loadData(); //this.form.loadData
            Fire.$on("refreshData", () => {
                this.loadData();
            });
        }
    };
</script>
