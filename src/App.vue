<script setup lang="ts">
import { RouterView } from 'vue-router';

import Header from '@/components/Header.vue';
import Footer from '@/components/Footer.vue';

import { nSQL } from 'nano-sql';

import jsonRegion from './json/Region.json';
import jsonCity from './json/City.json';
import jsonCounterparty from './json/Counterparty.json';
import jsonFinancialFlows from './json/FinancialFlows.json';
</script>

<template>
   <div class="d-flex flex-column min-vh-100">
      <Header title="Цифровая платформа социально-экономического развития моногородов России" />
      <RouterView />
      <Footer title="Разработано" />
   </div>
</template>

<script lang="ts">
export default {
   name: 'AppView',
   data() {
      return {};
   },
   beforeCreate() {
      nSQL('region').model([
         { key: 'id', type: 'int', props: ['pk', 'ai'] },
         { key: 'Name', type: 'string', default: '' },
      ]);

      nSQL('city').model([
         { key: 'id', type: 'int', props: ['pk', 'ai'] },
         { key: 'IDRegion', type: 'int', default: null },
         { key: 'Name', type: 'string', default: '' },
         { key: 'Enterprise', type: 'string', default: '' },
      ]);

      nSQL('counterparty').model([
         { key: 'id', type: 'int', props: ['pk', 'ai'] },
         { key: 'IDCity', type: 'int', default: null },
         { key: 'Name', type: 'string', default: '' },
         { key: 'Abbreviation', type: 'string', default: '' },
      ]);

      nSQL('financialFlows').model([
         { key: 'id', type: 'int', props: ['pk', 'ai'] },
         { key: 'IDCounterparty', type: 'int', default: null },
         { key: 'Name', type: 'string', default: '' },
         { key: 'Abbreviation', type: 'string', default: '' },
         { key: 'Coming', type: 'int', default: 0 },
         { key: 'Care', type: 'int', default: 0 },
      ]);

      nSQL()
         .config({
            id: 'my_db',
            mode: 'PERM',
         })
         .connect()
         .then(() => {
            console.log('DB (' + 'my_db' + '): создан');
         });
   },
   created() {
      nSQL('region')
         .query('select')
         .exec()
         .then((value) => {
            console.log('DB (' + 'region' + '): проверка на загрузку JSON');
            if (value.length <= 0) {
               nSQL()
                  .loadJS('region', jsonRegion)
                  .then(() => {
                     console.log('DB (' + 'region' + '): загружен');
                  });
            }
         });

      nSQL('city')
         .query('select')
         .exec()
         .then((value) => {
            console.log('DB (' + 'city' + '): проверка на загрузку JSON');
            if (value.length <= 0) {
               nSQL()
                  .loadJS('city', jsonCity)
                  .then(() => {
                     console.log('DB (' + 'city' + '): загружен');
                  });
            }
         });

      nSQL('counterparty')
         .query('select')
         .exec()
         .then((value) => {
            console.log('DB (' + 'counterparty' + '): проверка на загрузку JSON');
            if (value.length <= 0) {
               nSQL()
                  .loadJS('counterparty', jsonCounterparty)
                  .then(() => {
                     console.log('DB (' + 'counterparty' + '): загружен');
                  });
            }
         });

      nSQL('financialFlows')
         .query('select')
         .exec()
         .then((value) => {
            console.log('DB (' + 'financialFlows' + '): проверка на загрузку JSON');
            if (value.length <= 0) {
               nSQL()
                  .loadJS('financialFlows', jsonFinancialFlows)
                  .then(() => {
                     console.log('DB (' + 'financialFlows' + '): загружен');
                  });
            }
         });
   },
};
</script>

<style></style>
