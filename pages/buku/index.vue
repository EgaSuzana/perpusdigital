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
          
          <div v-for="(book,i) in books" :key="i" class="col-2">
            <div class="card mb-2 ">
              <div class="card-body">
                <nuxt-link :to="`/buku/${book.id}`">
                  <img :src="book.cover" alt="" width="170" height="230"> <h6>{{ book.judul }}</h6>
                </nuxt-link>
              </div>
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
.card-body{
  width: 400px;
  object-fit: cover;
  object-position: 0 30;
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