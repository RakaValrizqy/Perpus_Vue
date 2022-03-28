<template>
    <div>
        <div class="container-fluid px-4">
            <h1 class="mt-4">Member</h1>
            <ol class="breadcrumb mb-4">
                <router-link to="/" class="nav-link">
                    <div class="sb-nav-link-icon"><i class="fas fa-columns"></i></div>
                    home
                </ol>

            <div class="card mb-4">
                <div class="card-header">
                    <i class="fas fa-table me-1"></i>
                    List Member

                    <button @click="Add()" data-bs-toggle="modal" data-bs-target="#member_modal" class="btn btn-primary btn-sm float-end"><i class="fas fa-plus fa-fw"></i> Add Member</button>
            </div>
            <div class="card mb-4">
                <div class="card-body">
                    <table id="datatablesSimple" class="table table-hover table-striped">
                        <thead>
                            <tr>
                                <th>ID</th>
                                <th>NAMA</th>
                                <th>TANGGAL LAHIR</th>
                                <th>GENDER</th>
                                <th>ALAMAT</th>
                                <th>KELAS</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr v-for="ls in list_siswa" :key="ls">
                                <td>{{ls.id}}</td>
                                <td>{{ls.nama_siswa}}</td>
                                <td>{{ls.tanggal_lahir}}</td>
                                <td>{{ls.gender}}</td>
                                <td>{{ls.alamat}}</td>
                                <td>{{ls.nama_kelas}}</td>
                            </tr>
                        </tbody>
                    </table>
                </div>
            </div>
        </div>
    </div>

    <!-- Member Modal -->
        <div class="modal fade" id="member_modal" tabindex="-1" aria-labelledby="staticBackdropLabel" aria-hidden="true">
            <div class="modal-dialog modal-lg">
                <div class="modal-content">
                    <div class="modal-header">
                        <h5 class="modal-title" id="staticBackdropLabel">Member Data</h5>
                        <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                    </div>
                    <div class="modal-body">
                        <div class="mb-3">
                            <label for="nama_siswa" class="form-label">Nama siswa</label>
                            <input type="text" class="form-control" id="nama_siswa" v-model="nama_siswa" placeholder="Nama Siswa">
                        </div>

                        <div class="mb-3">
                            <label for="tanggal_lahir" class="form-label">Tanggal_lahir</label>
                            <input type="date" class="form-control" id="tanggal_lahir" v-model="tanggal_lahir" placeholder="tanggal_lahir">
                        </div>

                        <div class="mb-3">
                            <label for="gender" class="form-label">Gender</label>
                            <select name="gender" v-model="gender" class="form-control">
                                <option></option>
                                <option v-for="gender in listGender" :key="gender.key" v-bind:value="gender.key">{{gender.val}}</option>
                            </select>
                        </div>

                        <div class="mb-3">
                            <label for="alamat" class="form-label">alamat</label>
                            <textarea class="form-control" id="alamat" v-model="alamat" rows="3"></textarea>
                        </div>

                        <div class="mb-3">
                            <label for="kelas" class="form-label">Kelas</label>
                            <select name="kelas" v-model="kelas" class="form-control">
                                <option></option>
                                <option v-for="kelas in listKelas" :key="kelas.id_kelas" v-bind:value="kelas.id_kelas">{{kelas.nama_kelas}}</option>
                            </select>
                        </div>

                    </div>
                    <div class="modal-footer">
                        <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Cancel</button>
                        <button type="button" class="btn btn-primary" @click="Save()" data-bs-dismiss="modal">Submit</button>
                    </div>
                </div>
            </div>
        </div>
</template>
<script>
    module.exports = {
        //state
        data : function(){
            return {
                id:"",
                nama_siswa:"",
                tanggal_lahir:"",
                gender:"",
                alamat:"",
                kelas:"",
                action:"",
                list_siswa:[],
                listKelas:[],
                listGender:[
                    {key:"L", val:"Laki-laki"},
                    {key:"P", val:"Perempuan"}
                ]
            }
        },
        methods: {
            getData: function(){
                //token
                let token ={
                    headers : {"Authorization" : "Bearer" + this.$cookies.get("Authorization")}
                }
                axios.get(api_url + "/siswa", token)
                .then(response=>{
                    this.list_siswa = response.data;
                })
            },
            getKelas: function(){
                let token = {
                    headers : {"Authorization" : "Bearer" + this.$cookies.get("Authorization")}
                }
                axios.get(api_url + "/kelas", token)
                .then(response=>{
                    console.log(response)
                    this.listKelas=response.data;
                })
            },
            Add: function(){
                this.id="",
                this.nama_siswa="",
                this.tanggal_lahir="",
                this.gender="",
                this.alamat="",
                this.kelas="",
                this.action = "insert"
            },
            Save: function(){
                //token
                let token ={
                    headers : {"Authorization" : "Bearer" + this.$cookies.get("Authorization")}
                }
                //mapping data
                let form = {
                    //backend       //state
                    'nama_siswa': this.nama_siswa,
                    'tanggal_lahir': this.tanggal_lahir,
                    'gender': this.gender,
                    'alamat': this.alamat,
                    'id_kelas': this.kelas
                }
                if(this.action === 'insert'){   //post
                    axios.post(api_url + "/siswa", form, token)
                    .then(response=>{
                        Swal.fire({
                        title: 'Success!',
                        text: response.data.message,
                        icon: 'success',
                        confirmButtonText: 'Cool'
                    })
                    })
                } else {    //put
                    
                }
                this.getData()
            }
        },
        mounted() {
            this.getData()
            this.getKelas()
        },
    }
</script>