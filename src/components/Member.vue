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
            </div>
            <div class="card mb-4">
                <div class="card-body">
                    <table id="datatablesSimple" class="table table-hover table-striped">
                        <thead>
                            <tr>
                                <th>ID MEMBER</th>
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
                list_siswa:[],
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
                    console.log(response);
                    this.list_siswa = response.data;
                })
            }
        },
        mounted() {
            this.getData()
        },
    }
</script>