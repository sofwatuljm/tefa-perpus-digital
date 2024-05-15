<template>
  <div class="container-fluid">
    <div class="row">
      <div class="col-lg-12">
        <h2 class="text-center my-4">ISI BUKU KUNJUNGAN</h2>
        <form @submit.prevent="kirimData">
          <div class="mb-3">
            <input v-model="form.nama" type="text" class="form-control form-control-lg rounded-5 style= background-color : #4B7CAA" placeholder="NAMA.." />
          </div>
          <div class="mb-3">
            <select v-model="form.keanggotaan" class="form-control form-control-lg select rounded-5">
              <option value="">KEANGGOTAAN</option>
              <option v-for="(member, i) in members" :key="i" :value="member.id">{{ member.nama }}</option>
            </select>
          </div>
          <div class="mb-3" v-if="form.keanggotaan == '3'">
            <select v-model="form.tingkat" class="form-control form-control-lg form select rounded-5 mb-2">
              <option value="">TINGKAT</option>
              <option value="X">X</option>
              <option value="XI">XI</option>
              <option value="XII">XII</option>
            </select>

            <div class="mb-3">
              <select v-model="form.jurusan" class="form-control form-control-lg form-select rounded-5 mb-2">
                <option value="">JURUSAN</option>
                <option value="PPLG">PPLG</option>
                <option value="TJKT">TJKT</option>
                <option value="TSM">TSM</option>
                <option value="DKV">DKV</option>
                <option value="TOI">TOI</option>
              </select>
            </div>
            <div class="mb-3">
              <select v-model="form.kelas" class="form-control form-control-lg form-select rounded-5 mb-2">
                <option value="">KELAS</option>
                <option value="1">1</option>
                <option value="2">2</option>
                <option value="3">3</option>
                <option value="4">4</option>
              </select>
            </div>
          </div>
          <div class="mb-3">
            <select v-model="form.keperluan" class="form-control form-control-lg form-select rounded-5">
              <option value="">KEPERLUAN</option>
              <option v-for="(item, i) in objectives" :key="i" :value="item.id">{{ item.nama }}</option>
            </select>
          </div>
          <div class="row p-2">
            <div class="col-7">
              <button class="btn bg-primary btn-lg text-dark rounded-5 px-5" type="submit">KIRIM</button>
            </div>
            <div class="col-5 d-flex justify-content-end">
              <nuxt-link to="/" class="btn btn-warning btn-lg text-dark rounded-5 px-5"> KEMBALI </nuxt-link>
            </div>
          </div>
        </form>
      </div>
    </div>
  </div>
</template>

<style scoped>
.btn-warning {
  background-color: #4baa53 !important;
}
.bg-primary {
  background-color: #4b7caa !important;
}
</style>

<script setup>
useHead({
  title:"PERPUS DIGITAL",
  meta: [
    {
      name:"description",
      content:"halaman form pengunjung",
    }
  ]
})
const supabase = useSupabaseClient();

const members = ref([]);
const objectives = ref([]);

const form = ref({
  nama: "",
  keanggotaan: "",
  tingkat: "",
  jurusan: "",
  kelas: "",
  keperluan: "",
});

const kirimData = async () => {
  // console.log(form.value)
  const { error } = await supabase.from("pengunjung").insert([form.value]);
  if (!error) navigateTo("/pengunjung");
};

const getKeanggotaan = async () => {
  const { data, error } = await supabase.from("keanggotaan").select(" * ");
  if (data) members.value = data;
};

const getKeperluan = async () => {
  const { data, error } = await supabase.from("keperluan").select(" * ");
  if (data) objectives.value = data;
};

onMounted(() => {
  getKeanggotaan();
  getKeperluan();
});
</script>
