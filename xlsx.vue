<template>
  <div class="test">
    <div class="upload">
      <input type="file" @change="transform">
    </div>
    <div class="excel">
      <div class="col" v-for="(item, index) in data" :key="index">
        <div class="item" v-for="itm in item" :key="itm">
          {{itm}}
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import XLSX from 'xlsx'
export default {
  data () {
    return {
      data: []
    }
  },
  methods: {
    transform () {
      let file = event.target.files[0]
      this._file(file)
    },
    _file (file) {
      const reader = new FileReader()
      reader.onload = (e) => {
        const bstr = e.target.result
        const wb = XLSX.read(bstr, {type: 'binary'})
        const wsname = wb.SheetNames[0]
        const ws = wb.Sheets[wsname]
        const data = XLSX.utils.sheet_to_json(ws, {header: 1})
        this.data = data
      }
      reader.readAsBinaryString(file)
    }
  }
}
</script>

<style scoped>
  .upload{
    margin: 50px auto;
    text-align: center;
  }
  .excel{
    margin: 20px auto;
    width: 1400px;
    border-top: 1px solid rgba(0, 0, 0, 0.1);
    border-left: 1px solid rgba(0, 0, 0, 0.1);
  }
  .col{
    border-bottom: 1px solid rgba(0, 0, 0, 0.1);
    display: flex;
    align-items: center;
  }
  .item{
    flex: 1;
    height: 50px;
    border-right: 1px solid rgba(0, 0, 0, 0.1);
    display: flex;
    align-items: center;
    justify-content: center;
  }
</style>
