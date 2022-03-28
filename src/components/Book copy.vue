<template>
    <div>
        <div class="container-fluid px-4">
            <h1 class="mt-4">Book</h1>
            <ol class="breadcrumb mb-4">
                <li class="breadcrumb-item"><a href="index.html">Dashboard</a></li>
                <li class="breadcrumb-item active">Book</li>
            </ol>
            
            <div class="card mb-4">
                <div class="card-header">
                    <i class="fas fa-table me-1"></i>
                    List Book

                    <button @click="Add()" data-bs-toggle="modal" data-bs-target="#book_modal" class="btn btn-primary btn-sm float-end"><i class="fas fa-plus fa-fw"></i>Add Book</button>
                </div>
                <div class="card-body">
                    <table id="datatablesSimple" class="table table-hover table stripped">
                        <thead>
                            <tr>
                                <th>BOOK ID</th>
                                <th>BOOK NAME</th>
                                <th>AUTHOR</th>
                                <th>DESC</th>
                                <th>ACTION</th>
                            </tr>
                        </thead>
                        <tfoot>
                            <tr>
                                <th>BOOK ID</th>
                                <th>BOOK NAME</th>
                                <th>AUTHOR</th>
                                <th>DESC</th>
                                <th>ACTION</th>
                            </tr>
                        </tfoot>
                        <tbody>
                            <tr v-for="lb in list_book" :key="lb">
                                <td>{{ lb.id }}</td>
                                <td>{{ lb.nama_buku }}</td>
                                <td>{{ lb.pengarang }}</td>
                                <td>{{ lb.deskripsi }}</td>
                                <td>
                                    <button class="btn btn-info"><i class="fas fa-pencil-alt"></i></button>
                                    <button class="btn btn-danger"><i class="fas fa-trash-alt"></i></button>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </div>
            </div>
        </div>
        <!-- Modal -->
        <div class="modal fade" id="book_modal" data-bs-backdrop="static" data-bs-keyboard="false" tabindex="-1" aria-labelledby="staticBackdropLabel" aria-hidden="true">
            <div class="modal-dialog modal-lg">
                <div class="modal-content">
                    <div class="modal-header">
                        <h5 class="modal-title" id="staticBackdropLabel">Modal title</h5>
                        <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                    </div>
                <div class="modal-body">
                    <div class="mb-3">
                        <label for="nama_buku" class="form-label">Book name</label>
                        <input type="text" class="form-control" id="nama_buku" v-model="nama_buku" placeholder="Book Name">
                    </div>

                    <div class="mb-3">
                        <label for="pengarang" class="form-label">Author</label>
                        <input type="text" class="form-control" id="pengarang" v-model="pengarang" placeholder="Author">
                    </div>

                    <div class="mb-3">
                        <label for="deskripsi" class="form-label">Description</label>
                        <textarea class="form-control" id="deskripsi" rows="3"></textarea>
                    </div>
                </div>
                    <div class="modal-footer">
                        <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Close</button>
                        <button type="button" class="btn btn-primary" @click="Save()" data-bs-dismiss="modal">Submit</button>
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
            id : "",
            nama_buku : "",
            pengarang : "",
            deskripsi: "",
            action : "",
            message : "",
            list_book : [],
        }
    },
    methods: {
        getData: function(){
            //token
            let config = {
            headers : {
              "Authorization" : "Bearer " + this.$cookies.get('Authorization')
            }
          }
          axios.get(api_url + "/buku", config)
          .then(response => {
              this.list_book = response.data;
          })
        },
        Add: function(){
            this.id_buku = ""
            this.nama_buku = ""
            this.pengarang = ""
            this.deskripsi = ""
            this.action = "insert"
        },
        Save: function() {
            //mapping header token
            let config = {
            headers : {
              "Authorization" : "Bearer " + this.$cookies.get("Authorization")
            }
          }
            //mapping data
            let form  = {
                //backend       //state
                'nama_buku': this.nama_buku,
                'pengarang': this.pengarang,
                'deskirpsi': this.deskripsi
            }
            if(this.action === 'insert'){ //POST
                axios.post(api_url + '/buku', form, config)
                .then( response => {
                    alert("Success Add Book!")
                })
                
            } else { //PUT
            
            }
        }
    },
    mounted() {
        this.getData()
    }
}
</script>