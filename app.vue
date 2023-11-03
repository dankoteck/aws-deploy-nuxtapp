<script setup lang="ts">
const route = useRoute()
const router = useRouter()

const options = [
  { value: 'Hello', label: 'Hello' },
  { value: 'World', label: 'World' },
  { value: 'Goodbye', label: 'Goodbye' },
]

const selected = ref('Hello')

watch(
  () => route.fullPath,
  () => {
    const query = route.query.selected as string

    if (query || query !== '') {
      const idx = options.findIndex(({ value }) => value === query)
      if (idx > -1) {
        selected.value = query
      }
    } else {
      throw createError({
        statusCode: 404,
        message: 'Page not found',
        fatal: true
      })
    }
  },
  { immediate: true }
)

watch(selected, (currentVal) => {
  router.push(`?selected=${currentVal}`)
})
</script>

<template>
  <div>
    <p>{{ selected }}</p>

    <form>
      <select v-model="selected">
        <option v-for="option in options" :key="option.value" :value="option.value">
          {{ option.label }}
        </option>
      </select>
    </form>
  </div>
</template>

<style scoped>
div {
  width: 100%;
  height: 100vh;

  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}

form {
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>