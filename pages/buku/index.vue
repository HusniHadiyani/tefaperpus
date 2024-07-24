<script setup>
const supabase = useSupabaseClient();

const books = ref([]);
const Semua = ref(0);
const keyword = ref('');

const getBooks = async () => {
  const { data, error } = await supabase.from("buku").select(`*, kategori_buku(*)`)
  .ilike('judul', `%${keyword.value}%`)
  if (data) books.value = data;
};

const getSemuabuku = async () => {
  const { data, count } = await supabase.from("buku").select("*", { count: "exact" });
  if (data) Semua.value = count;
};

onMounted(() => {
  getBooks()
  getSemuabuku()
});
</script>

<template>
  <div class="bg">
    <div class="row">
      <div class="col-lg-12">
        <h2 class="text-center text-white my-4">BUKU</h2>
        <div class="my-3">
          <form @submit.prevent="getBooks">
            <input v-model="keyword" type="search" class="form-control rounded-5 bi bi-search" placeholder="Cari buku disini......" />
          </form>
        </div>
      </div>
      <div class="my-3 text-white">menampilkan {{ books.length }} dari {{ Semua }}</div>
      <div class="row">
        <div v-for="(book, i) in books" :key="i" class="col-lg-2">
          <div class="card-mb-3">
            <div class="card-body">
              <nuxt-link :to="`/Buku/${book.id}`">
                <img :src="book.cover" class="cover" alt="cover" />
                <h6>{{ book.judul }}</h6>
              </nuxt-link>
            </div>
          </div>
        </div>
      </div>
    </div>
    <nuxt-link to="/">
      <button type="submit" class="btn btn-lg rounded-5 px-5 pl-5">Menu</button>
    </nuxt-link>
  </div>
</template>

<style scoped>
.bg {
  background-image: url(assets/img/yy.webp);
  height: 100;
  background-position: center;
  background-repeat: repeat;
  background-size: cover;
}
.card-body {
  width: 100%;
  height: em;
  padding: 0;
}
.cover {
  width: 100%;
  height: 100%;
  object-fit: cover;
  object-position: 0 30;
}

.btn {
  background-color: rgb(14, 154, 235);
}
</style>