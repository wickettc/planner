<template>
    <div class="calendar-container">
        <div class="calendar-selectors">
            <select v-model="month" id="select-month">
                <option value="0">January</option>
                <option value="1">February</option>
                <option value="2">March</option>
                <option value="3">April</option>
                <option value="4">May</option>
                <option value="5">June</option>
                <option value="6">July</option>
                <option value="7">August</option>
                <option value="8">September</option>
                <option value="9">October</option>
                <option value="10">November</option>
                <option value="11">December</option>
            </select>
            <div class="selcted-date-div">
                {{ selectedDate ? selectedDate.getDate() : '' }}
            </div>
            <select v-model="year" id="select-year">
                <option v-for="n in yearArr" :key="n" :value="n">{{
                    n
                }}</option>
            </select>
        </div>
        <div class="calendar">
            <table>
                <thead>
                    <tr>
                        <th v-for="(column, index) in tbHeader" :key="index">
                            {{ column }}
                        </th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="(weekArr, ind) in tbData" :key="ind">
                        <td
                            v-on="weekArr[i] ? { click: handleHighlight } : {}"
                            :class="[
                                // sets all days (with numbers) to be clickable
                                weekArr[i] && 'clickable',
                                // compares today to the iterable date to set today
                                today.toLocaleDateString() ===
                                new Date(
                                    year,
                                    month,
                                    weekArr[i]
                                ).toLocaleDateString()
                                    ? 'today'
                                    : '',
                            ]"
                            v-for="(day, i) in weekArr"
                            :key="i"
                        >
                            {{ weekArr[i] }}
                        </td>
                    </tr>
                </tbody>
            </table>
        </div>
    </div>
</template>

<script>
export default {
    name: 'Calendar',
    data() {
        return {
            yearArr: [],
            tbHeader: ['Sun', 'Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat'],
            tbData: [],
            year: undefined,
            month: undefined,
            highlightedDay: undefined,
            selectedDate: undefined,
            today: new Date(),
        };
    },
    created() {
        let today = new Date();
        this.year = today.getFullYear();
        this.month = today.getMonth();
        // lists out year range for select dropdown
        for (let i = 1990; i < 2100; i++) {
            this.yearArr.push(i);
        }
    },
    watch: {
        year: function() {
            this.clearCalendar();
            this.createCalendar();
        },
        month: function() {
            this.clearCalendar();
            this.createCalendar();
        },
        selectedDate: function() {
            this.$emit('selected-date', this.selectedDate.toLocaleDateString());
        },
    },
    methods: {
        createCalendar() {
            let curMonth = new Date(this.year, this.month).getMonth();
            let dayCount = 1;
            let cellCount = 0;
            let tbArrCount = 0;
            let startCal = true;
            let keepLoop = true;
            while (keepLoop) {
                let date = new Date(this.year, this.month, dayCount);
                if (date.getMonth() === curMonth) {
                    // start new array for each "week"
                    if (cellCount === 0) {
                        this.tbData.push([]);
                        tbArrCount++;
                    }
                    // add blank spaces to begining if week does not start on sun
                    if (startCal && date.getDay() !== 0) {
                        let addSpaces = date.getDay();
                        for (let i = 0; i < addSpaces; i++) {
                            this.tbData[tbArrCount - 1].push(null);
                        }
                        startCal = false;
                        cellCount = addSpaces;
                    } else {
                        startCal = false;
                    }
                    // pushes "days" to tbData array
                    this.tbData[tbArrCount - 1].push(date.getDate());
                    dayCount++;
                    // starts new array (week) once we hit a len of 7
                    if (cellCount === 6) {
                        cellCount = 0;
                    } else {
                        cellCount++;
                    }
                    // ends this month
                } else {
                    // only sets today as selected date on init
                    if (!this.selectedDate) {
                        this.selectedDate = this.today;
                    }
                    keepLoop = false;
                }
            }
        },
        clearCalendar() {
            this.tbData = [];
            if (this.highlightedDay) {
                this.highlightedDay.classList.remove('highlight');
            }
        },
        handleHighlight(e) {
            if (this.highlightedDay) {
                this.highlightedDay.classList.remove('highlight');
            }
            e.target.classList.add('highlight');
            this.highlightedDay = e.target;
            let date = new Date(this.year, this.month, e.target.innerHTML);
            this.selectedDate = date;
        },
    },
};
</script>

<style scoped>
.calendar-container {
    display: inline-block;
    font-size: 1.2rem;
    height: 100%;
}

.calendar-show-btn {
    width: 100%;
}

.calendar-selectors {
    box-sizing: border-box;
    display: flex;
    justify-content: space-evenly;
    align-items: center;
    width: 100%;
    padding: 2px;
    height: 15%;
    font-size: 1.5rem;
    border-bottom: 1px solid gray;
    background: #a7f5a7;
}

.calendar-selectors select {
    font-size: 1.4rem;
}

.calendar {
    box-sizing: border-box;
    width: 100%;
    height: 85%;
}

.calendar table {
    width: 100%;
    height: 100%;
    box-sizing: border-box;
    table-layout: fixed;
    border-spacing: 0;
}

select {
    border: none;
}

.clickable {
    cursor: pointer;
}

.today {
    background-color: #a7f5a7;
    color: black;
    box-sizing: border-box;
}

.highlight {
    background-color: rgb(143, 140, 140);
    color: white;
    box-sizing: border-box;
}

td {
    text-align: center;
    box-sizing: border-box;
}

select,
.selcted-date-div {
    background: #a7f5a7;
    border: 1px solid white;
    padding: 8px 10px;
    color: black;
    border-radius: 5%;
}

@media only screen and (max-width: 767px) {
    td {
        padding: 8px 0;
    }
}
</style>
