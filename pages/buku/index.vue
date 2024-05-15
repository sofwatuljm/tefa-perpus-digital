<template>
  <div class="container-fluid">
    <div class="row">
      <div class="col-lg-12">
        <h2 class="text-center my-4">CARi BUKU</h2>
        <div class="my-3">
          <form @submit.prevent="getBooks">
            <input v-model="keyword" type="search" class="form-control rounded-5" placeholder="Mau baca apa hari ini?" />
          </form>
        </div>

        <div class="my-3 text-muted text-white">Menampilkan {{ books.length }} dari {{ jumlah }}</div>
        <div class="row">
          <div v-for="(book,i) in books" :key="i" class="col-lg-2">
            <nuxt-link :to="`buku/${book.id}`">
              <div class="card mb-3">
                <div class="card-body">
                  <img :src="book.cover" class="cover" alt="cover"/>
                </div>
              </div>
            </nuxt-link>
          </div>
        </div>

      </div>
    </div>
  </div>

  <div class="d-flex justify-content-end">
    <nuxt-link to="/" class="btn bg-primary btn-lg text-dark rounded-5 px-5"> KEMBALI </nuxt-link>
  </div>
</template>

<script setup>
useHead({
  title:"PERPUS DIGITAL",
  meta: [
    {
      name:"description",
      content:"halaman detail buku",
    }
  ]
})
const supabase = useSupabaseClient()

const books = ref([])

const keyword = ref('')

const jumlah = ref([])

const getBooks = async () => {
  const { data, error } = await supabase.from("buku").select(`*,kategori(*)`)
  .ilike('judul', `%${keyword.value}%`)
  if (data) books.value = data
}

const jumlahBuku = async () => {
  const { data, count } = await supabase.from("buku").select(`*`, {count: "exact"});
  if (data) jumlah.value = count;
}

onMounted(() => {
  getBooks();
  jumlahBuku();
});


</script>

<style scoped>
.card-body {
  width: 100%;
  height: 20 rem;
  padding: 0;
}

.cover {
  width: 100%;
  height: 100%;
  object-fit: cover;
  object-position: 0;
}
.bg-primary {
  background-color: #4baa53 !important;
}
</style>
