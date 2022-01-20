<template>
  <q-layout view="lHh Lpr lFf">
    <q-header elevated>
      <q-toolbar>
        <q-toolbar-title>
          Challenge
        </q-toolbar-title>

        <div>
          <q-btn
            flat
            :to="{name:'Login'}"
            dense
            round
            aria-label="Menu"
            icon="logout"
          />
        </div>
      </q-toolbar>
    </q-header>

    <q-page-container>
      <!-- <HelloWorld /> -->
      <!-- <router-view/> -->
      <div class="container__content">
        <div class="left shadow-5">
          <div>
            <q-select
              color="indigo"
              filled
              v-model="rangeShifts.model"
              :options="rangeShifts.options"
              :label="rangeShifts.label"
            >
              <template v-slot:prepend>
                <q-icon name="event" />
              </template>
            </q-select>
          </div>
          <div class="item__shift"
          v-for="shift in shifts" :key="shift.key" @click="selectedShift(shift)">{{shift.title}}</div>
        </div>
        <!-- Right -->
        <div class="right">
          <q-card class="my-card text-white shadow-5" v-if="details.title !== ''">
              <q-card-section style="background: radial-gradient(circle, #35a2ff 0%, #014a88 100%)">
                <div class="text-h6">{{ details.title }}</div>
                <div class="text-subtitle2">{{ details.name }}</div>
              </q-card-section>

              <q-card-section>
                <div class="q-pa-md" style="max-width: 350px">
                  <q-list>
                    <q-item>
                      <q-item-section>
                        <q-item-label style="color:black">Office</q-item-label>
                        <q-item-label caption lines="2">{{ details.office }}</q-item-label>
                      </q-item-section>
                      <q-item-section side top><q-icon name="star" color="indigo" /></q-item-section>
                    </q-item>
                    <q-separator spaced inset />
                    <q-item>
                      <q-item-section>
                        <q-item-label style="color:black">Age</q-item-label>
                        <q-item-label caption lines="2">{{ details.age }}</q-item-label>
                      </q-item-section>
                      <q-item-section side top><q-icon name="star" color="indigo" /></q-item-section>
                    </q-item>
                    <q-separator spaced inset />
                    <q-item>
                      <q-item-section>
                        <q-item-label style="color:black">Hiring Date</q-item-label>
                        <q-item-label caption lines="2">{{ details.hiringDate }}</q-item-label>
                      </q-item-section>
                      <q-item-section side top><q-icon name="star" color="indigo" /></q-item-section>
                    </q-item>
                    <q-separator spaced inset />
                    <q-item>
                      <q-item-section>
                        <q-item-label style="color:black">Hobbies</q-item-label>
                        <q-item-label caption lines="2">{{ details.hobbies }}</q-item-label>
                      </q-item-section>
                      <q-item-section side top><q-icon name="star" color="indigo" /></q-item-section>
                    </q-item>
                    <q-separator spaced inset />
                    <q-item>
                      <q-item-section>
                        <q-item-label style="color:black">Description</q-item-label>
                        <q-item-label caption lines="2">{{ details.description }}</q-item-label>
                      </q-item-section>
                      <q-item-section side top><q-icon name="star" color="indigo" /></q-item-section>
                    </q-item>
                    <q-separator spaced inset />
                  </q-list>
                </div>
              </q-card-section>
          </q-card>
          <div v-else>
            <q-card class="my-card">
              <q-card-section>
                Please select one of the shifts on the left
              </q-card-section>
            </q-card>
          </div>
        </div>
      </div>
    </q-page-container>
  </q-layout>
</template>

<script>
// @ is an alias to /src
// import HelloWorld from '@/components/HelloWorld.vue'
import { onMounted, toRefs, watch, reactive } from 'vue'
import { date } from 'quasar'

export default {
  name: 'Home',
  components: {
    // HelloWorld
  },
  setup () {
    /** To use i18n on setup, not necessary on data(not composition), in data only need us $t to access i18n */
    const state = reactive({
      rangeShifts: {
        label: 'Select the day range',
        model: 2,
        options: [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
      },
      shifts: [],
      details: {
        title: '',
        name: '',
        age: '',
        office: '',
        hiringDate: '',
        hobbies: '',
        shifts: 0,
        description: ''
      }
    })
    /**
     * Methods
     */
    const generateStartShifts = () => {
      const currentDate = Date.now()
      const currentYear = date.formatDate(currentDate, 'YYYY')
      const currentMonth = date.formatDate(currentDate, 'MM')
      const currentDay = date.formatDate(currentDate, 'DD')
      const dateEarlyStart = date.buildDate({ year: currentYear, month: currentMonth, date: currentDay, hours: 7, minutes: 0, seconds: 0 })
      const dateEarlyEnd = date.buildDate({ year: currentYear, month: currentMonth, date: currentDay, hours: 14, minutes: 0, seconds: 0 })
      const dateLateStart = date.buildDate({ year: currentYear, month: currentMonth, date: currentDay, hours: 15, minutes: 0, seconds: 0 })
      const dateLateEnd = date.buildDate({ year: currentYear, month: currentMonth, date: currentDay, hours: 22, minutes: 0, seconds: 0 })
      const newShiftLate = {
        date: currentDate,
        title: date.formatDate(currentDate, 'YYYY-MM-DD') + ' 23:00:00 - late',
        content: {
          name: 'Name of person',
          age: 'birthday',
          office: 'job',
          hiringDate: getHiringDate(),
          hobbies: 'something you like to do',
          description: 'Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip.'
        }
      }
      const newShiftNight = {
        date: currentDate,
        title: date.formatDate(currentDate, 'YYYY-MM-DD') + ' 07:00:00 - night',
        content: {
          name: 'Name of person',
          age: 'birthday',
          office: 'job',
          hiringDate: getHiringDate(),
          hobbies: 'something you like to do',
          description: 'Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip.'
        }
      }
      if (date.isBetweenDates(currentDate, dateEarlyStart, dateEarlyEnd)) {
        state.shifts.push(newShiftLate)
        state.shifts.push(newShiftNight)
        console.log(state.shifts)
      }
      if (date.isBetweenDates(currentDate, dateLateStart, dateLateEnd)) {
        state.shifts.push(newShiftNight)
      }
    }

    const generateNextShifts = () => {
      const newDate = Date.now()
      // const formattedDate = date.formatDate(newDate, 'YYYY-MM-DD HH:mm:ss')
      let clonedDate = date.clone(newDate)
      for (let i = 1; i <= state.rangeShifts.model; i++) {
        clonedDate = date.addToDate(clonedDate, { days: i })
        const newShiftEarly = {
          date: clonedDate,
          title: date.formatDate(clonedDate, 'YYYY-MM-DD') + ' 15:00:00 - early',
          content: {
            name: 'Name of person',
            age: 'birthday',
            office: 'job',
            hiringDate: getHiringDate(),
            hobbies: 'something you like to do',
            description: 'Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip.'
          }
        }
        const newShiftLate = {
          date: clonedDate,
          title: date.formatDate(clonedDate, 'YYYY-MM-DD') + ' 23:00:00 - late',
          content: {
            name: 'Name of person',
            office: 'job',
            age: 'birthday',
            hiringDate: getHiringDate(),
            hobbies: 'something you like to do',
            description: 'Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip.'
          }
        }
        const newShiftNight = {
          date: clonedDate,
          title: date.formatDate(clonedDate, 'YYYY-MM-DD') + ' 07:00:00 - night',
          content: {
            name: 'Name of person',
            age: 'birthday',
            office: 'job',
            hiringDate: getHiringDate(),
            hobbies: 'something you like to do',
            description: 'Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip.'
          }
        }
        state.shifts.push(newShiftEarly)
        state.shifts.push(newShiftLate)
        state.shifts.push(newShiftNight)
      }
    }

    const selectedShift = (shift) => {
      state.details.title = shift.title
      state.details.name = shift.content.name
      state.details.office = shift.content.office
      state.details.age = shift.content.age
      state.details.hiringDate = shift.content.hiringDate
      state.details.hobbies = shift.content.hobbies
      state.details.description = shift.content.description
    }

    const getLeftColumn = () => {
      generateStartShifts()
      generateNextShifts()
    }

    const getHiringDate = () => {
      return `${Math.floor(Math.random() * (28 - 1)) + 1}-${Math.floor(Math.random() * (12 - 1)) + 1}${Math.floor(Math.random() * (1984 - 2000)) + 1}`
    }

    const init = () => {
      state.shifts = []
      getLeftColumn()
      return true
    }
    watch(
      () => state.rangeShifts.model,
      () => {
        init()
      }
    )
    onMounted(() => {
      init()
    })
    return { ...toRefs(state), init, selectedShift, getLeftColumn }
  }
}
</script>
<style lang="scss">
.container__content {
  width: 100%;
  display: flex;
  margin-top: 30px;
  padding: 20px;
  &>div { padding: 30px; }
  .left {
    .item__shift {
      font-size: 1.2em;
      margin: 10px 0;
      transition: ease 0.4ms;
      cursor: pointer;
      &:hover { color: indigo; }
    }
  }
  .right {
    // to do
  }
}
</style>
