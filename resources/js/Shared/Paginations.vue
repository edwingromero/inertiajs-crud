<template>

  <nav aria-label="...">
    <ul class="pagination justify-content-end">
      <template v-for="(link, key) in links">
        <li v-if="link.url === null" :key="key" class="page-item disabled" :class="{ 'ml-auto': link.label === 'Next' }">
          <a class="page-link" href="#" tabindex="-1" aria-disabled="true"  v-html="link.label"></a>
        </li>
        <li v-else :key="key" class="page-item" :class="{'active': link.active}">

          <inertia-link  class="page-link" :class="{'ml-auto': link.label === 'Next' }" :href="link.url" v-html="link.label"></inertia-link>
        </li>
      </template>
    </ul>

  </nav>


</template>

<script>
export default {
  props: {
    links: Array,
  },
}
</script>