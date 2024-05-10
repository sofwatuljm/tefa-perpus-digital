<template>
    <div class="container-fluid">
        <div class="row my-5">
            <div class="col-lg-6">
                <nuxt-link to="/pengunjung/tambah">
                    <div class="card bg-pengunjung rounded-5 mb-5">
                        <div class="card-body">
                            <h2>Pengunjung</h2>
                        </div>
                    </div>  
                </nuxt-link>
            </div>

            <div class="col-lg-6">
                <nuxt-link to="/buku">
                    <div class="card bg-buku rounded-5 ">
                        <div class="card-body">
                            <h2>Cari Buku</h2>
                        </div>
                    </div>
                </nuxt-link>
            </div>
            <div class="statistik rounded-5 mb-7">
            <h1 class="pt-5 ps-0">STATISTIK</h1>
            </div>

            <div class="col-lg-6">
                <nuxt-link to="/pengunjung">
                <div class="card pengunjung rounded-5 mt-5 mb-7">
                    <div class="card-body text">
                        <h1 class="ps-5"> {{ banyakP }} </h1>
                        <h2 class="pt-5"> Pengunjung</h2>
                    </div>
                </div>
            </nuxt-link>
            </div>

            <div class="col-lg-6">
                <!-- <nuxt-link to="/buku"> -->
                <div class="card buku rounded-5 mt-5">
                    <div class="card-body text">
                        <h1 class="ps-5"> {{ banyakB }} </h1>
                        <h2 class="pt-5"> Buku </h2>
                    </div>
                </div>
            <!-- </nuxt-link> -->
            </div>
        </div>
        <Chart />
    </div>
</template>

<script setup>
const supabase = useSupabaseClient();

const banyakP = ref(0);

const banyakB =  ref(0);

const pengunjung = async () => {
    const{data,error,count} = await supabase.from('pengunjung').select('*', {count: 'exact'})
    if(count) banyakP.value = count;
};

const buku = async () => {
    const{data,error,count} = await supabase.from('buku').select('*', {count: 'exact'})
    if (count) banyakB.value = count;
};

onMounted(() =>{
    pengunjung();
    buku();
})
</script>

<style scoped>
.card {
    height: 250px;
    box-shadow: 1px 1px 10px #424242;
}

.card.bg-pengunjung {
    background-image: url("../assets/img/bg-kunjungan.jpeg");
    background-repeat: no-repeat;
    background-position: center center;
    background-size: cover;
    color: black;
    opacity: 50%;
}

.card.bg-buku {
    background: url("../assets/img/bg-buku.jpg") no-repeat center center;
    background-size: cover;
    color: black;
    opacity: 50%;
}

.card.pengunjung {
    background-color: #EDE76E;

}
.card.buku {
    background-color: #6EEDB7;
}
.text{
    display: flex;
    align-items: center;
}
.text >h1 {
    font-size: 7rem;
}
</style>
