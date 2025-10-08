<template>
  <div class="q-pa-md" style="max-width: 400px">

    <q-form
      @submit="onSubmit"
      @reset="onReset"
      class="q-gutter-md"
    >
      <q-input
        filled
        v-model="name"
        label="Your name *"
        hint="Name and surname"
        lazy-rules
        :rules="[ val => val && val.length > 0 || 'Please type something']"
      />

      <q-input
        filled
        type="number"
        v-model="age"
        label="Your age *"
        lazy-rules
        :rules="[
          val => val !== null && val !== '' || 'Please type your age',
          val => (val !== null && val > 0 && val < 100) || 'Please type a real age'
        ]"
      />

      <q-toggle v-model="accept" label="I accept the license and terms" />

      <div>
        <q-btn label="Submit" type="submit" color="primary"/>
        <q-btn label="Reset" type="reset" color="primary" flat class="q-ml-sm" />
      </div>
    </q-form>

  </div>
</template>

<script lang="ts">
import { useQuasar } from 'quasar'
import type { QVueGlobals } from 'quasar' // แก้ไข: ใช้ import type สำหรับประเภท
import { ref, defineComponent } from 'vue'
import type { Ref } from 'vue' // แก้ไข: ใช้ import type สำหรับประเภท

export default defineComponent({
  name: 'IndexPage',
  setup () {
    // กำหนดประเภทให้กับ $q
    const $q: QVueGlobals = useQuasar()

    // กำหนดประเภทให้กับ Ref เพื่อความปลอดภัยของ TypeScript
    const name: Ref<string | null> = ref(null)
    const age: Ref<number | null> = ref(null)
    const accept: Ref<boolean> = ref(false)

    const onSubmit = () => {
      if (accept.value !== true) {
        $q.notify({
          color: 'red-5',
          textColor: 'white',
          icon: 'warning',
          message: 'You need to accept the license and terms first'
        })
      }
      else {
        // age.value อาจเป็น null หรือ number ได้
        const ageValue = age.value !== null ? age.value.toString() : 'N/A'

        $q.notify({
          color: 'green-4',
          textColor: 'white',
          icon: 'cloud_done',
          message: `Submitted! Name: ${name.value}, Age: ${ageValue}`
        })
      }
    }

    const onReset = () => {
      name.value = null
      age.value = null
      accept.value = false
    }

    return {
      name,
      age,
      accept,
      onSubmit,
      onReset
    }
  }
})
</script>

<style scoped>
/* สไตล์ที่จำเป็น */
</style>
