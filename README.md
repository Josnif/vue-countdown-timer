# Vue Countdown Timer

## Usage

To set countdown for a defined numbers of `days`, `hours`, `minutes` or `seconds`. Use the type props

```html
<template>
    <CountdownTimer :type="'days'" :dayText="'days'" :endTime="3" />
</template>

<script>
import CountdownTimer from './components/CountdownTimer.vue';

export default {
  name: 'App',
  components: {
    CountdownTimer
  },
}
</script>
```

#### Default for the type props is `date`.

```html
<template>
    <CountdownTimer :endTime="new Date('2023-04-25 00:00:00')" />
</template>
```

#### You can also customize the template

```html
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
```

#### Full Usage

```html
<template>
    <CountdownTimer 
        :startTime="new Date('2023-04-25 00:00:00')" 
        :endTime="new Date('2023-05-20 00:00:00')" 
        :endText="'Event Ended'"
        :dayText="'days'"
        :hourText="'h'"
        :minuteText="'m'"
        :secondText="'s'"
        @start-callback="handleCountdownStart"
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
    
    const handleCountdownStart = () => {
      console.log('Countdown start!');
    }

    return {
        handleCountdownEnd, handleCountdownStart
    }

  }
}
</script>
```

## Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```