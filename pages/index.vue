<template>
  <div class="text-center grid w-full gap-1.5 md:grid-cols-2">
      <AdminCard />
      <MemberCard />
  </div>
</template>

<script>
import AdminCard from "@/components/AdminCard"
import MemberCard from "@/components/MemberCard"

export default {
  components: {
    AdminCard,
    MemberCard
  },
  data () {
    return {
    }
  },
  methods: {
 }
}
</script>
