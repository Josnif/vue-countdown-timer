<template>
  <CountdownTimer :type="'days'" :dayText="'days'" :endTime="3" />
  <CountdownTimer :type="'days'" :dayText="'days'" :endTime="3">
    <template v-slot:countdown="{ days, hours, minutes, seconds }">
      <div class="">
        <div v-if="days > 0" class="text-2xl font-bold">{{ days }} days</div>
        <div class="text-4xl font-bold">
          <span v-if="days > 0">{{ hours.toString().padStart(2, '0') }}:</span>
          {{ minutes.toString().padStart(2, '0') }}:{{ seconds.toString().padStart(2, '0') }}
        </div>
      </div>
    </template>
  </CountdownTimer>
  <CountdownTimer 
    :endTime="new Date('2023-04-25 00:00:00')" 
    :endText="'Event Ended'"
    :dayText="'days'"
    @end-callback="handleCountdownEnd"
  />
  
</template>

<script>
import CountdownTimer from './components/CountdownTimer.vue';

export default {
  name: 'App',
  components: {
    CountdownTimer
  },
  setup() {
    const handleCountdownEnd = () => {
      console.log('Countdown ended!');
    }

    return {
      handleCountdownEnd
    }

  }
}
</script>

<style>

</style>
