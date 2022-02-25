<script setup lang="ts">
import { nSQL } from 'nano-sql';
</script>

<template>
  <main>
    <div class="container">
      <h2 class="text-center">Главная</h2>

      <h3 class="text-center">
        <small class="text-muted">Основная страница SPA-приложения</small>
      </h3>

      <h3>
        <small class="text-muted">Субъект РФ: ({{ region.length }})</small>
      </h3>

      <template v-if="region.length > 0">
        <select class="form-select form-select-lg bg-dark text-light mb-3" aria-label="Выбор субъекта РФ" v-model="regionSelect">
          <option value="null" disabled>Выберите субъект РФ:</option>
          <template v-for="(value, index) in region" :key="value.id">
            <option :value="[value.id, index, value.Name]">{{ value.Name }}</option>
          </template>
        </select>
      </template>
      <template v-else>
        <h4>
          <small class="text-muted">Нет базы данных</small>
        </h4>
      </template>

      <h3>
        <small class="text-muted">Населённый пункт: ({{ city.length }})</small>
      </h3>

      <template v-if="city.length > 0">
        <select class="form-select form-select-lg bg-dark text-light mb-3" aria-label="Выбор населённого пункта" v-model="citySelect">
          <option value="null" disabled selected>Выберите населённый пункт:</option>
          <template v-for="(value, index) in city" :key="value.id">
            <option :value="[value.id, index, value.Name]">{{ value.Name }}</option>
          </template>
        </select>
      </template>
      <template v-else>
        <h4>
          <small class="text-muted">Выберите субъект РФ</small>
        </h4>
      </template>

      <h3>
        <small class="text-muted">Контрагент: ({{ сounterparty.length }})</small>
      </h3>

      <template v-if="сounterparty.length > 0">
        <select class="form-select form-select-lg bg-dark text-light mb-3" aria-label="Выбор контрагента" v-model="counterpartySelect">
          <option value="null" disabled selected>Выберите контрагента:</option>
          <template v-for="(value, index) in сounterparty" :key="value.id">
            <option :value="[value.id, index, value.Name]">{{ value.Name }}</option>
          </template>
        </select>
      </template>
      <template v-else>
        <h4>
          <small class="text-muted">Выберите населённый пункт</small>
        </h4>
      </template>

      <template v-if="financialFlows.length > 0">
        <h2 class="text-center">Финансовые потоки</h2>

        <h3 class="text-center">
          <small class="text-muted">Детальная информация финансовых потоков по: {{ counterpartySelectName }}</small>
        </h3>

        <div class="table-responsive">
          <table class="table caption-top table-dark table-bordered table-striped table-hover">
            <caption>
              Таблица 1 - Входящие и исходящие финансовые потоки и их доля по контрагенту «{{
                counterpartySelectName
              }}»
            </caption>
            <thead class="text-center align-middle">
              <tr>
                <th scope="col">#</th>
                <th scope="col" @click="sort('Name')">Наименование показателя</th>
                <th scope="col" @click="sort('Coming')">Входящие потоки (млн. руб.)</th>
                <th scope="col" @click="sort('Care')">Исходящие потоки (млн. руб.)</th>
                <th scope="col">Сальдо (млн. руб.)</th>
              </tr>
            </thead>
            <tbody class="align-middle">
              <template v-if="financialFlows.length > 0">
                <template v-for="(value, index) in financialFlows" :key="value.id">
                  <template v-if="value.Name !== counterpartySelectName">
                    <tr>
                      <th class="text-center" scope="row">{{ index }}</th>
                      <td class="text-left">{{ value.Name }}</td>
                      <td class="text-center">
                        {{ value.Coming }} ({{ ((value.Coming * 99.99) / totalComing).toFixed(2) }}
                        %)
                      </td>
                      <td class="text-center">
                        {{ value.Care }} ({{ ((value.Care * 99.99) / totalCare).toFixed(2) }}
                        %)
                      </td>
                      <th class="text-center">{{ value.Coming - value.Care }}</th>
                    </tr>
                  </template>
                </template>
              </template>
              <template v-else>
                <tr>
                  <td class="text-center" colspan="5">Нет данных</td>
                </tr>
              </template>
            </tbody>
            <tfoot class="align-middle">
              <tr>
                <th class="text-center" scope="row">#</th>
                <th class="text-left">Итого</th>
                <th class="text-center">{{ totalComing }}</th>
                <th class="text-center">{{ totalCare }}</th>
                <th class="text-center">{{ totalComing - totalCare }}</th>
              </tr>
            </tfoot>
          </table>
        </div>

        <h2 class="text-center">Капитал</h2>

        <h3 class="text-center">
          <small class="text-muted">Детальная информация капитала по: {{ counterpartySelectName }}</small>
        </h3>

        <div class="table-responsive">
          <table class="table caption-top table-dark table-bordered table-striped table-hover">
            <caption>
              Таблица 2 - Расчёт сальдо по контрагенту «{{
                counterpartySelectName
              }}»
            </caption>
            <thead class="text-center align-middle">
              <tr>
                <th scope="col">#</th>
                <th scope="col">Наименование показателя</th>
                <th scope="col">{{ counterpartySelectName }}</th>
              </tr>
            </thead>
            <tbody class="align-middle">
              <tr>
                <th class="text-center" scope="row">0</th>
                <td class="text-left">Входящие потоки (млн. руб.)</td>
                <td class="text-center">{{ totalComing }}</td>
              </tr>
              <tr>
                <th class="text-center" scope="row">1</th>
                <td class="text-left">Исходящие потоки (млн. руб.)</td>
                <td class="text-center">{{ totalCare }}</td>
              </tr>
            </tbody>
            <tfoot class="align-middle">
              <tr>
                <th class="text-center" scope="row">#</th>
                <th class="text-left">Сальдо (млн. руб.)</th>
                <th class="text-center">{{ totalComing - totalCare }}</th>
              </tr>
            </tfoot>
          </table>
        </div>
      </template>
      <template v-else>
        <h4>
          <small class="text-muted">Выберите контрагента</small>
        </h4>
      </template>
    </div>
  </main>
</template>

<script lang="ts">
export default {
  name: 'HomeView',
  data() {
    return {
      region: [] as Array<any>,
      regionSelect: null as any,
      regionSelectName: '',
      city: [] as Array<any>,
      citySelect: null as any,
      citySelectName: '',
      сounterparty: [] as Array<any>,
      counterpartySelect: null as any,
      counterpartySelectName: '',
      financialFlows: [] as Array<any>,
      financialFlowsSelect: null as any,
      currentSort: 'Name',
      currentSortDir: 'asc',
    };
  },
  computed: {
    totalComing() {
      let temp: any = this.financialFlows;
      return temp.reduce((acc: any, cur: any) => acc + Number(cur.Coming), 0);
    },
    totalCare() {
      let temp: any = this.financialFlows;
      return temp.reduce((acc: any, cur: any) => acc + Number(cur.Care), 0);
    },
  },
  methods: {
    sort(s: any) {
      if (s === this.currentSort) {
        this.currentSortDir = this.currentSortDir === 'asc' ? 'desc' : 'asc';
      }
      this.currentSort = s;
      let modifier = 1;
      if (this.currentSortDir === 'desc') modifier = -1;

      return this.financialFlows.sort(function (a, b) {
        if (a[s] < b[s]) {
          return -1 * modifier;
        }
        if (a[s] > b[s]) {
          return 1 * modifier;
        }
        return 0;
      });
    },
    async regionData() {
      await nSQL('region')
        .query('select')
        .exec()
        .then((rows) => {
          this.region = rows;
        });
    },
  },
  watch: {
    async regionSelect(array: string) {
      localStorage.setItem('region', array);
      await nSQL('city')
        .query('select')
        .where(['IDRegion', '=', Number(array[0])])
        .exec()
        .then((rows) => {
          this.city = rows;
        });
      this.regionSelectName = array[2];
    },
    async citySelect(array: string) {
      localStorage.setItem('city', array);
      await nSQL('counterparty')
        .query('select')
        .where(['IDCity', '=', Number(array[0])])
        .exec()
        .then((rows) => {
          this.сounterparty = rows;
        });
      this.citySelectName = array[2];
    },
    async counterpartySelect(array: string) {
      localStorage.setItem('counterparty', array);
      await nSQL('financialFlows')
        .query('select')
        .where(['IDCounterparty', '=', Number(array[0])])
        .exec()
        .then((rows) => {
          this.financialFlows = rows;
        });
      this.counterpartySelectName = array[2];
    },
  },
  mounted() {
    this.regionData();

    if (localStorage.getItem('region')) {
      this.regionSelect = String(localStorage.getItem('region')).split(',');
    }

    if (localStorage.getItem('city')) {
      this.citySelect = String(localStorage.getItem('city')).split(',');
    }

    if (localStorage.getItem('counterparty')) {
      this.counterpartySelect = String(localStorage.getItem('counterparty')).split(',');
    }
  },
  unmounted() {
    nSQL('region')
      .disconnect()
      .then(() => {
        console.log('DB (' + 'region' + '): остановлено');
      });

    nSQL('city')
      .disconnect()
      .then(() => {
        console.log('DB (' + 'city' + '): остановлено');
      });

    nSQL('counterparty')
      .disconnect()
      .then(() => {
        console.log('DB (' + 'counterparty' + '): остановлено');
      });

    nSQL('financialFlows')
      .disconnect()
      .then(() => {
        console.log('DB (' + 'financialFlows' + '): остановлено');
      });
  },
};
</script>

<style scoped></style>
