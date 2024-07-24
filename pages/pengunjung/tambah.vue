<template>
  <div class="container-fluid">
    <div class="row">
        <div class="col-lg-12">
          <h2 class="text-center my-4">ISI BUKU KUNJUNGAN</h2>
          <form @submit.prevent="kirimData">
          <div class="mb-3">
            <input v-model="form.nama" type="text" class="form-control form-control-lg rounded-3" placeholder="Nama.....">
          </div>
          <div class="mb-3">
            <select v-model="form.keanggotaan" class="form-control form-control-lg form-select rounded-3">
              <option value="">Keanggotaan</option>
              <option v-for="(member,i) in members" :key="i" :value="member.id">{{ member.nama }}</option>
            </select>
          </div>
          <div class="mb-3" v-if="form.keanggotaan == '1'">
            <div class="row">
              <div class="col-md-4">
                <select v-model="form.tingkat" class="form-control form-control-lg form-select rounded-3 mb-2">
                  <option value="">Tingkat</option>
                  <option value="X">X</option>
                  <option value="XI">XI</option>
                  <option value="XII">XII</option>
                </select> 
              </div>
              <div class="col-md-4">
                <select v-model="form.jurusan" class="form-control form-control-lg form-select rounded-3 mb-2">
                  <option value="">Jurusan</option>
                  <option value="pplg">pplg</option>
                  <option value="tjkt">tjkt</option>
                  <option value="tsm">tsm</option>
                  <option value="dkv">dkv</option>
                  <option value="toi">toi</option>
                  
                </select>
              </div>
              <div class="col-md-4">
                <select v-model="form.kelas" class="form-control form-control-lg form-select rounded-3 mb-2">
                  <option value="">Kelas</option>
                  <option value="1">1</option>
                  <option value="2">2</option>
                  <option value="3">3</option>
                  <option value="4">4</option>
                </select>
                </div>
              </div>
            </div>
            <div class="col-mb-3">
                <select v-model="form.keperluan" class="form-control form-control-lg form-select rounded-3 mb-4">
                  <option value="">Keperluan</option>
                  <option v-for="(item,i) in objectives" :key="i" :value="item.id">{{ item.nama }}</option>
                </select>
          </div>
          <button type="submit" class="btn btn-dark rounded-3 px-5">kirim</button>
          </form>
      </div>
    </div>
  </div>
</template>

<script setup>
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
})

const kirimData = async () => {
  // console.log(form.value);
  const { data, error } = await supabase
    .from('pengunjung')
    .insert([form.value])
    .select()
  if(data) {
    navigateTo('/pengunjung')
    console.log('success')
  }
  if(error) throw error
};

const getKeanggotaan = async () => {
  const { data, error } = await supabase.from("keanggotaan").select("*");
  if (data) members.value = data;
};

const getKeperluan = async () => {
  const { data, error } = await supabase.from("keperluan").select("*");
  if (data) objectives.value = data;
  // console.log(objectives.value);
};

onMounted(() => {
  getKeanggotaan();
  getKeperluan();
});
</script>