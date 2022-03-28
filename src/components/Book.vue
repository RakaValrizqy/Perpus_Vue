<template>
    <div>
       <div class="container-fluid px-4">
            <h1 class="mt-4">Book</h1>
            <ol class="breadcrumb mb-4">
                <router-link to="/" class="nav-link">
                                <div class="sb-nav-link-icon"><i class="fas fa-columns"></i></div>
                                home
                
            </ol>

            <div class="card mb-4">
                <div class="card-header">
                    <i class="fas fa-table me-1"></i>
                    List Book

                    <button @click="Add()" data-bs-toggle="modal" data-bs-target="#book_modal" class="btn btn-primary btn-sm float-end"><i class="fas fa-plus fa-fw"></i> Add Book</button>
                </div>
                <div class="card-body">
                    <table id="datatablesSimple" class="table table-hover table-striped">
                        <thead>
                            <tr>
                                <th>ID BUKU</th>
                                <th>COVER</th>
                                <th>NAMA BUKU</th>
                                <th>PENGARANG</th>
                                <th>DESKRIPSI</th>
                                <th>ACTION</th>
                            </tr>
                        </thead>
                        
                        <tbody>
                            <tr v-for="lb in list_book" :key="lb">
                                <td>{{ lb.id }}</td> 
                                <td><img :src="'http://localhost:8000/images/' + lb.cover" width="150"></td>
                                <td>{{ lb.nama_buku }}</td>
                                <td>{{ lb.pengarang }}</td>
                                <td>{{ lb.deskripsi }}</td>
                                <td>
                                    <button class="btn btn-default" @click="Edit(lb)" data-bs-toggle="modal" data-bs-target="#cover_modal" ><i class="fas fa-image fa-fw"></i></button>
                                    <button class="btn btn-info" @click="Edit(lb)" data-bs-toggle="modal" data-bs-target="#book_modal" ><i class="fas fa-pencil-alt fa-fw"></i></button>
                                    <button class="btn btn-danger" @click="Delete(lb.id)"><i class="fas fa-trash-alt fa-fw"></i></button>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </div>
            </div>
        </div>

        <!-- Modal -->
        <div class="modal fade" id="book_modal" tabindex="-1" aria-labelledby="staticBackdropLabel" aria-hidden="true">
            <div class="modal-dialog modal-lg">
                <div class="modal-content">
                    <div class="modal-header">
                        <h5 class="modal-title" id="staticBackdropLabel">Book Data</h5>
                        <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                    </div>
                    <div class="modal-body">
                        <div class="mb-3">
                            <label for="nama_buku" class="form-label">Nama Buku</label>
                            <input type="text" class="form-control" id="nama_buku" v-model="nama_buku" placeholder="Book Name">
                        </div>

                        <div class="mb-3">
                            <label for="pengarang" class="form-label">Pengarang</label>
                            <input type="text" class="form-control" id="pengarang" v-model="pengarang" placeholder="pengarang">
                        </div>

                        <div class="mb-3">
                            <label for="deskripsi" class="form-label">Deskripsi</label>
                            <textarea class="form-control" id="deskripsi" v-model="deskripsi" rows="3"></textarea>
                        </div>

                    </div>
                    <div class="modal-footer">
                        <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Cancel</button>
                        <button type="button" class="btn btn-primary" @click="Save()" data-bs-dismiss="modal">Submit</button>
                    </div>
                </div>
            </div>
        </div>

        <div class="modal fade" id="cover_modal" tabindex="-1" aria-labelledby="staticBackdropLabel" aria-hidden="true">
            <div class="modal-dialog modal-lg">
                <div class="modal-content">
                    <div class="modal-header">
                        <h5 class="modal-title" id="staticBackdropLabel">Cover</h5>
                        <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                    </div>
                    <div class="modal-body">
                        <div class="mb-3">
                            <label for="cover" class="form-label">Cover</label>
                            <input type="file" class="form-control" id="cover" @change="uploadCover($event)">
                        </div>

                    </div>
                    <div class="modal-footer">
                        <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Cancel</button>
                        <button type="button" class="btn btn-primary" @click="Upload(id)" data-bs-dismiss="modal">Submit</button>
                    </div>
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
            id: "",
            nama_buku: "",
            pengarang: "",
            deskripsi: "",
            cover:"",
            action: "",
            list_book: [],
        }
    },
    methods: {
        getData: function(){
            //token
            let token = {
                headers : { "Authorization" : "Bearer " + this.$cookies.get("Authorization")}
            }
            axios.get(api_url + "/buku", token)
            .then( response => {
                this.list_book = response.data;
            })
        },
        Add: function() {
            this.id = ""
            this.nama_buku = ""
            this.pengarang = ""
            this.deskripsi = ""
            this.action = "insert"
        },
        Edit: function(lb){
            this.id = lb.id
            this.nama_buku = lb.nama_buku
            this.pengarang = lb.pengarang
            this.deskripsi = lb.deskripsi
            this.action = "update"
        },
        uploadCover: function(e){
            this.cover = e.target.files[0]
        },
        Upload: function(id){
            let token = {
                headers : { 
                    "Authorization" : "Bearer " + this.$cookies.get("Authorization"), 
                    'Content-Type' : 'multipart/form-data',
                }
            }
            let form  = new FormData()
            form.append("cover", this.cover)
            axios.post(api_url + '/buku/uploadCover/'+ id, form, token)
            .then( response => {
                Swal.fire({
                    title: 'Success!',
                    text: response.data.message,
                    icon: 'success',
                    confirmButtonText: 'OK'
                })
                this.getData()
            })
        },
        Save: function() {
            //mapping header token
            let token = {
                headers : { "Authorization" : "Bearer " + this.$cookies.get("Authorization")}
            }
            //mapping data
            let form  = {
                //backend       //state
                'nama_buku': this.nama_buku,
                'pengarang': this.pengarang,
                'deskripsi': this.deskripsi,
                'cover': this.cover
            }
            if(this.action === 'insert'){ //POST
                axios.post(api_url + '/buku', form, token)
                .then( response => {
                    Swal.fire({
                        title: 'Success!',
                        text: response.data.message,
                        icon: 'success',
                        confirmButtonText: 'Cool'
                    })
                })
          } else { //PUT
                axios.put(api_url + '/buku/' + this.id, form, token)
                .then( response => {
                    Swal.fire({
                        title: 'Success!',
                        text: response.data.message,
                        icon: 'success',
                        confirmButtonText: 'Cool'
                    })
                })
            }
            this.getData()
        },
        Delete: function(id){
            //mapping header token
            let token = {
                headers : { "Authorization" : "Bearer " + this.$cookies.get("Authorization")}
            }

            Swal.fire({
                title: 'Hapus Data Buku',
                text: 'Apakah anda yakin menghapus data ini?',
                icon : 'warning',
                showDenyButton: true,
                showCancelButton: false,
                confirmButtonText: 'Ya',
                denyButtonText: `Tidak`,
            }).then((result) => {
            /* Read more about isConfirmed, isDenied below */
            if (result.isConfirmed) {
                axios.delete(api_url + '/buku/' + id, token)
                .then( response => {
                    Swal.fire({
                        title: 'Success!',
                        text: response.data.message,
                        icon: 'success',
                        confirmButtonText: 'Cool'
                    })
                    this.getData()
                })

            } else if (result.isDenied) {
                Swal.fire({
                    title: 'Batal!',
                    text: 'Data tidak jadi dihapus',
                    icon: 'error',
                    confirmButtonText: 'Cool'
                })
                }
            })
        }
    },
    mounted() {
        this.getData()
    },
}
</script>