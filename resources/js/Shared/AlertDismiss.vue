<template>
  <div>
    <b-alert :show="dismissCountDown" variant="info" @dismiss-count-down="countDownChanged">
        {{message}}
    </b-alert>
  </div>
</template>

<script>
export default {
  props:['message','count'],
  created() {
    this.countDownChanged(this.count)
  },
  data() {
    return {
       dismissCountDown: null,
    }
  },
  methods: {
    countDownChanged(segundos) {
      this.dismissCountDown = segundos;
    }
  },
}
</script>