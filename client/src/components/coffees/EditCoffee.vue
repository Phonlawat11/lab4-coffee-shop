<template>
  <div>
    <h1>Edit Coffee</h1>

    <div v-if="coffee">

      <p>
        Name:
        <input v-model="coffee.name" type="text" />
      </p>

      <p>
        Price:
        <input v-model.number="coffee.price" type="number" />
      </p>

      <p>
        Type:
        <select v-model="coffee.type">
          <option value="hot">hot</option>
          <option value="iced">iced</option>
          <option value="frappe">frappe</option>
        </select>
      </p>

      <p>
        Status:
        <select v-model="coffee.status">
          <option value="มีจำหน่าย">มีจำหน่าย</option>
          <option value="หมด">หมด</option>
        </select>
      </p>

      <p>
        Description:
        <textarea v-model="coffee.description"></textarea>
      </p>

      <div v-if="coffee.image" style="margin-bottom:10px;">
        <p>รูปปัจจุบัน:</p>
        <img
          :src="`http://localhost:8081/assets/uploads/${coffee.image}`"
          style="width:120px;height:120px;border-radius:10px;object-fit:cover;"
        />
      </div>

      <upload-image @uploaded="onUploaded" />

      <br />

      <button @click="updateCoffee">บันทึกการแก้ไข</button>
      <button @click="$router.push('/coffees')" style="margin-left: 10px;">กลับ</button>

    </div>

    <div v-else>
      Loading...
    </div>
  </div>
</template>

<script>
import CoffeesService from '../../services/CoffeesService'
import UploadImage from '../Utils/Upload.vue'

export default {
  components: {
    UploadImage
  },

  data () {
    return {
      coffee: null
    }
  },

  async created () {
    const coffeeId = this.$route.params.coffeeId
    this.coffee = (await CoffeesService.show(coffeeId)).data
  },

  methods: {
    onUploaded (filename) {
      this.coffee.image = filename
    },

    async updateCoffee () {
      try {
        await CoffeesService.put(this.coffee)
        this.$router.push('/coffees')
      } catch (err) {
        console.log(err)
      }
    }
  }
}
</script>