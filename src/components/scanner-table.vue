<template>
  <div>
    <table class="scanner-table">
      <thead>
        <tr>
          <th>ODS</th>
          <th>Fita</th>
          <th>Etiqueta</th>
          <th>Aparicions</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(d, i) in datum" :key="i">
          <td data-label="Topic">{{d.topic}}</td>
          <td data-label="Subtopic">{{d.subtopic}}</td>
          <td data-label="Etiqueta">{{d.tag}}</td>
          <td data-label="Ocurrencias">{{d.times}}</td>
        </tr>
      </tbody>
    </table>
    <tipi-message
      v-if="totalRows > limitRows"
      type="info"
      icon>Estem mostrant {{limitRows}} d'un total de {{totalRows}} resultats trobats.</tipi-message>
  </div>
</template>

<script>
import { TipiMessage } from 'tipi-uikit';

export default {
  name: 'scanner-table',
  components: {
    TipiMessage,
  },
  data() {
    return {
      datum: [],
      totalRows: 0,
      limitRows: 10,
    };
  },
  created() {
    this.parseResults();
  },
  props: {
    result: {
      type: Object || null,
      required: true,
    },
  },
  methods: {
    parseResults() {
      /**
      * Map tags array to custom array
      */
      if (!this.result.tags) return;

      const totaltags = this.result.tags
        .reduce((cnt, o) => (cnt + o.times), 0);

      this.totalRows = this.result.tags.length;

      this.datum = this.result.tags.map(d => {
        const item = { ...d };
        item.percent = Math.floor((d.times / totaltags)*100);
        return item;
      }).sort(d => d.times).slice(0, this.limitRows);
    },
  },
};
</script>

<style lang="scss">
.scanner-table {
  .number {
    text-align: right;
  }
}
</style>
