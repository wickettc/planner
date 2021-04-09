<template>
    <div class="calendar-container">
        <div class="calendar-selectors">
            <select v-model="year" id="select-year">
                <option v-for="n in yearArr" :key="n" :value="n">{{
                    n
                }}</option>
            </select>
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
        </div>
        <div id="calendar">
            <table>
                <thead>
                    <tr>
                        <th v-for="(column, index) in tbHeader" :key="index">
                            {{ column }}
                        </th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="(arr, ind) in tbData" :key="ind">
                        <td
                            v-on="arr[i] ? { click: handleHighlight } : {}"
                            :class="arr[i] && 'clickable'"
                            v-for="(d, i) in arr"
                            :key="i"
                        >
                            {{ arr[i] }}
                        </td>
                    </tr>
                </tbody>
            </table>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            yearArr: [],
            tbHeader: ['Sun', 'Mon', 'Tues', 'Wed', 'Thurs', 'Fri', 'Sat'],
            tbData: [],
            year: undefined,
            month: undefined,
            highlightedDay: undefined,
            selectedDate: undefined,
        };
    },
    created() {
        let today = new Date();
        this.year = today.getFullYear();
        this.month = today.getMonth();
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
                    if (cellCount === 0) {
                        this.tbData.push([]);
                        tbArrCount++;
                    }
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
                    this.tbData[tbArrCount - 1].push(date.getDate());
                    dayCount++;
                    if (cellCount === 6) {
                        cellCount = 0;
                    } else {
                        cellCount++;
                    }
                } else {
                    keepLoop = false;
                }
            }
        },
        clearCalendar() {
            this.tbData = [];
        },
        handleHighlight(e) {
            if (this.highlightedDay) {
                this.highlightedDay.classList.remove('highlight');
            }
            e.target.classList.add('highlight');
            this.highlightedDay = e.target;
            let date = new Date(
                this.year,
                this.month,
                e.target.innerHTML
            ).toLocaleDateString();
            this.selectedDate = date;
        },
    },
};
</script>

<style scoped>
.calendar-container {
    display: inline-block;
    border: 1px solid black;
    padding: 10px 5px;
    font-size: 1.2rem;
}

.calendar-selectors {
    display: flex;
    justify-content: space-evenly;
    width: 100%;
}

.clickable {
    cursor: pointer;
}

.highlight {
    background-color: black;
    color: white;
}

td {
    text-align: center;
}
</style>
