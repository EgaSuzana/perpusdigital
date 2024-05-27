<template>
  <div class="container-fluid">
    <div class="row">
      <div class="col-lg-12">
        <h2 class="text-center my-4"> CARI BUKU</h2>
        <div class="my-3">
          <form @submit.prevent="getBooks">
          <input v-model="keyword" type="search" class="form-control rounded-5" placeholder=" mau baca apa hari ini?">
        </form>
      </div>
        <div class="my-3 text-muted">Menampilkan {{ books.length }} dari {{ jumlah }}</div>
        <div class="row">
          
          <div v-for="(book,i) in books" :key="i" class="col-lg-2 mb-5">
            <div class="card mb-2 dcdc" >
              <div class="card-header d-flex justify-content-center">
                <nuxt-link :to="`/buku/${book.id}`">
                  <img :src="book.cover" alt="" width="170" height="230" class="rounded-10 buku"> 
                </nuxt-link>
              </div>
              <div class="card-body"><h6>{{ book.judul }}</h6></div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
  <nuxt-link to="/">
        <button type="submit" class="btn btn-dark btn-lg rounded- px-5">Kembali</button>
        </nuxt-link>
</template>

<style scoped>
.cover {
  width: 150%;
  height: 100%;
  object-fit: cover;
  object-position: 0 30;
}
.input{
  background-color: #D9D9D9;
}
.card-header{
  padding: 0%;
  object-fit: cover;
  object-position: 0 30;
}
.dcdc {
  width: 220px;
  height: 100%;
}

</style>
<script setup>
const supabase = useSupabaseClient()

const books = ref([])
const jumlah = ref(0)
const keyword = ref('')

const getBooks = async () => {
  const { data, error } = await supabase.from('buku').select(`*, kategori(*)`)
  .ilike('judul', `%${keyword.value}%`)
  if(data) books.value = data
}

const totalBuku = async () => {
  const { data, count } = await supabase.from('buku').select("*", { count: 'exact'})
  if (data) jumlah.value = count
}

onMounted(() => {
  getBooks()
  totalBuku()
})



</script>
