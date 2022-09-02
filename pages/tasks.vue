<template></template>

<script>
export default {
  head() {
    title: 'Todo Application - Tasks'
  },
  mounted: function () {
    const authToken = this.$cookie.get('auth-token')

    if (!authToken) {
      alert('SEM COOKIE')
      return window.location.replace('../')
    }

    this.$axios
      .get('http://localhost:8000/user', {
        headers: {
          Authorization: `Bearer ${authToken}`,
        },
      })
      .then((response) => {
        console.log(response.data)

        if (response.data.message === 'ACCESS_DENIED') {
          alert('NEGADO!')
          return window.location.replace('../')
        }

        alert('OK')
      })
  },
  methods: {},
}
</script>

<style></style>
