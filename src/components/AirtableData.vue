<template>
  <div>
    <div class="header">
      <h1>UPCOMING EVENTS</h1>
    </div>
    <div v-for="record in records" :key="record.id" class="event">
      <h4>
        {{ new Date(record.get('Date')).toLocaleDateString('en-US', {
            month: '2-digit',
            day: 'numeric',
            year: '2-digit'
          }) }}
      </h4>
      <div class="event-info">
        <h2>{{ record.get('Event Title') }}</h2>
        <p>{{ record.get('Long Description') }}</p>
      </div>
      <h3>Type</h3>
    </div>

  </div>
</template>

<script>
import Airtable from 'airtable';

const base = new Airtable({
  apiKey: process.env.VUE_APP_AIRTABLE_TOKEN
}).base(process.env.VUE_APP_AIRTABLE_BASE_ID);

export default {
  data() {
    return {
      records: []
    }
  },
  created() {
    base('Table 1').select({
      maxRecords: 10,
      view: 'Grid view'
    }).firstPage((err, records) => {
      if (err) { console.error(err); return; }
      this.records = records;
    });
  }
}
</script>

<style>
.header {
  text-align: right;
  padding: 20px 30px;
  background-color: #48C081;
  color: white;
}

h1 {
  all: unset;
  font-size: 65px;
  font-weight: 500;
  letter-spacing: -4px;
}

.event {
  padding: 28px 0;
  display: flex;
  justify-content: center;
  align-items: center;
  column-gap: 20px;
}

.event-info {
  text-align: left;
  width: 80ch;
}

h2 {
  all: unset;
  font-size: 20px;
  font-weight: bold;
}

h3 {
  all: unset;
  padding: 1px;
  border: solid 1px black;
  font-weight: 500;
}

h4 {
  all: unset;
  font-size: 18px;
  font-style: italic;
  letter-spacing: 1px;
}

p {
  all: unset;
  display: block;
  font-size: 16px;
}
</style>
  