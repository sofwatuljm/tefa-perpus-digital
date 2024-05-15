<template>
  <div class="container-fluid">
    <div class="row">
      <div class="col-lg-12">
        <h2 class="text-center my-4">RIWAYAT KUNJUNGAN</h2>
        <form @submit.prevent="getPengunjung">
          <div class="my-3">
            <input v-model="keyword" type="search" class="form-control form-control-lg rounded-5" placeholder="Filter..." />
          </div>
        </form>
        
        <div class="my-3 text-muted">menampilkan {{ visitors.length }} dari {{ jumlah }}</div>
        <table class="table">
          <thead>
            <tr>
              <td>NO</td>
              <td>NAMA</td>
              <td>KEANGGOTAAN</td>
              <td>WAKTU</td>
              <td>KEPERLUAN</td>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(visitor, i) in visitors" :key="i">
              <td>{{ i + 1 }}.</td>
              <td>{{ visitor.nama }}</td>
              <td>{{ visitor.keanggotaan.nama }}</td>
              <td>{{ visitor.tanggal }}. {{ visitor.waktu }}</td>
              <td>{{ visitor.keperluan.nama }}</td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
  <div class="col-5">
    <nuxt-link to="/" class="btn bg-primary btn-lg rounded-5 px-5 text-dark"> KEMBALI </nuxt-link>
  </div>
</template>

<script setup>
useHead({
  title:"PERPUS DIGITAL",
  meta: [
    {
      name:"description",
      content:"halaman riwayat kunjungan",
    }
  ]
})
const supabase = useSupabaseClient();

const keyword = ref([]);

const visitors = ref([]);

const jumlah = ref([]);

const getPengunjung = async () => {
  const { data, error } = await supabase.from("pengunjung")
  .select(`*, keanggotaan(*), keperluan(*)`)
  .ilike('nama', `%${keyword.value}`)
  .order("id", {ascending: false});
  if (data) visitors.value = data;
};

const jumlahPengunjung = async () => {
  const { data, count } = await supabase.from("pengunjung").select(`*`, {count: "exact"});
  if (data) jumlah.value = count;
}

onMounted(() => {
  getPengunjung();
  jumlahPengunjung();
});
</script>

<style scoped>
.bg-primary {
  background-color: #4baa53 !important;
}
</style>
