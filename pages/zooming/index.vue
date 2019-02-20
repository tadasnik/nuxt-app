<template>
  <section class="section">
    <div class="container" >
        <democomp></democomp>
    </div>
  </section>
</template>

<script>
import DemoComponent from '@/components/DemoComponent.vue'
export default{
    components: {
        democomp: DemoComponent
    }
}
</script>
