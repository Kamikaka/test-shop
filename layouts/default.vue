<template>
    <div>
        <Header />
        <main>
            <div class="container">
                <nuxt/>
            </div>
        </main>
    </div>
</template>

<script>
import Header from '~/components/Header.vue'
export default {
  components: { Header },
}
</script>