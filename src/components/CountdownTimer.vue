<template>
    <slot name="countdown" 
        :days="days" 
        :dayText="dayText" 
        :hours="hours" 
        :hourText="hourText"
        :minutes="minutes"
        :minuteText="minuteText"
        :seconds="seconds"
        :secondText="secondText"
    >
        <div class="flex items-center justify-center">
            <div v-if="!timerEnded" class="text-4xl font-bold flex">
                <span v-if="showDaysTime">{{ days }}</span>
                <i v-if="showDaysTime">{{ dayText }}</i>
    
                <span v-if="showHoursTime">{{ hours }}</span>
                <span v-if="showHoursTime">{{ hourText }}</span>
    
                <span> {{ minutes }}</span>
                <span> {{ minuteText }}</span>
    
                <span> {{ seconds }}</span>
                <span> {{ secondText }}</span>
            </div>
            <div class="text-4xl font-bold flex" v-else>{{ endText }}</div>
        </div>
    </slot>
</template>

<script>
import { computed, onMounted, onUnmounted, ref } from 'vue';
import moment from 'moment';

export default {
    name: 'CountdownTimer',
    props: {
        showDays: {
            type: Boolean,
            required: false
        },
        startTime: {
            type: [Date, String],
            required: false
        },
        endTime: {
            type: [Number, Date, String],
            required: true,
        },
        type: {
            type: String,
            required: true,
            default: 'date',
            validator: value => ['days', 'hours', 'minutes', 'seconds', 'date'].includes(value)
        },
        dayText: {
            type: String,
            default: ':'
        },
        hourText: {
            type: String,
            default: ':'
        },
        minuteText: {
            type: String,
            default: ':'
        },
        secondText: {
            type: String,
            default: ''
        },
        endText: {
            type: String,
            default: 'Time Ended'
        }
    },
    emits: ['start-callback', 'end-callback'],
    setup(props, { emit }) {
        const countdown = ref(0)

        if (props.type === 'days') {
            countdown.value = moment.duration(props.endTime, 'days').asSeconds()
        } else if (props.type === 'hours') {
            countdown.value = moment.duration(props.endTime, 'hours').asSeconds()
        } else if (props.type === 'minutes') {
            countdown.value = moment.duration(props.endTime, 'minutes').asSeconds()
        } else if (props.type === 'seconds') {
            countdown.value = moment.duration(props.endTime, 'seconds').asSeconds()
        } else {
            if (props.startTime && props.endTime) {
                countdown.value = moment(props.endTime).diff(moment(props.startTime), 'seconds')
            } else {
                const now = moment();
                const end = moment(props.endTime)
                countdown.value = end.diff(now, 'seconds')
            }
        }
 
        const days = computed(() => moment.duration(countdown.value, 'seconds').days());
        const hours = computed(() => moment.duration(countdown.value, 'seconds').hours());
        const minutes = computed(() => moment.duration(countdown.value, 'seconds').minutes());
        const seconds = computed(() => moment.duration(countdown.value, 'seconds').seconds());
        
        const showDaysTime = computed(() => (props.showDays || props.type === 'days') && days.value > 0);
        const showHoursTime = computed(() => hours.value > 0);
        const timerEnded = computed(() => (countdown.value <= 0 || isNaN(countdown.value) ));

        const countdownInterval = setInterval(() => {
            if (countdown.value > 0) {
                countdown.value--;
            } else {
                clearInterval(countdownInterval);
                emit('end-callback');
            }
        }, 1000)

        onMounted(() => {
            emit('start-callback')
        })

        onUnmounted(() => {
            clearInterval(countdownInterval)
        })

        return {
            days,
            hours,
            minutes,
            seconds,
            timerEnded,
            showDaysTime,
            showHoursTime,
        };
    }
}
</script>

<style>
.flex {
    display: flex;
}
.justify-center {
    justify-content: center;
}
.items-center {
    align-items: center;
}
.font-bold {
    font-weight: 700;
}
.text-4xl {
    font-size: 2.25rem;
    line-height: 2.5rem;
}

</style>