<template>
  <div>
    <section class="score">
      Player <span>{{ winCount }}</span> x <span>{{ loseCount }}</span> Computer
    </section>
  </div>
</template>

<script>
export default {
  /* eslint-disable */
  // eslint-disable-next-line
  name: 'Scoreboard',
  props: {
    winCount: Number,
    loseCount: Number
  }
}
</script>

<style lang="scss" scoped>
section.score {
  border-bottom: 1px solid black;
  padding: 24px;
  font-size: 18px;
}

section.score span {
  padding: 8px;
  font-weight: bold;
  border: 1px solid black;
}
</style>