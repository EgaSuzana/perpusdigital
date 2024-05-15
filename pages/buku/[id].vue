<template>
  <div class="container">
  <h2 class="text-start my-4">{{ buku.judul }}</h2>
    <div class="row">
      <div class="col-md-3">
        <img :src="buku.cover" class="cover" alt="buku" width="280px">
      </div>
      <div class="col-md-6">
        <div class="badge bg-primary p-2">{{ buku.kategori }}</div>
        <ul class="list-group list-group-flush">
          <li class="list-group-item">Penulis :  {{ buku.penulis }}</li>
          <li class="list-group-item">Tahun_terbit :  {{ buku.tahun_terbit }}</li>
          <li class="list-group-item">Penerbit :  {{ buku.penerbit }}</li>
          <li class="list-group-item">Deskripsi :  {{ buku.deskripsi }}</li>  
        </ul>
      </div>
      </div>
    </div>
    <div class="row m-2 p-5">
      <div class="col-lg-5">
        <nuxt-link to="/buku/">
        <button type="submit" class="btn btn-dark btn-lg rounded-5 px-5">Kembali</button>
        </nuxt-link>
      </div>
    </div>
    
</template>

<script setup>
import { ref, onMounted } from 'vue'
const supabase = useSupabaseClient()

const route = useRoute()
const buku = ref([null])

const getBookById = async () => {
  const { data, error } = await supabase.from('buku').select(`*, kategori(*)`)
    .eq('id', route.params.id)
  if (data) buku.value = data[0]
}


onMounted(async () => {
  await getBookById()
})
</script>