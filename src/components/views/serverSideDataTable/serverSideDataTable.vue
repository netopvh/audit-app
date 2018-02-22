<template>
  <div>
    <div class="layout-padding">
      <button class="primary raised fit" @click="$v.$touch()">Cadastrar Secretaria</button>
      <br><br>
      <div class="card">
        <div class="card-title bg-teal text-white">Lista de Secretarias</div>
        <div class="card-content">
          <div class="flex wrap gutter">
            <div class="auto">

              <q-search placeholder="Localizar Secretaria" :debounce="500"
                        v-model.lazy="searchBeer" @input="getBeers"></q-search>
            </div>
            <div class="auto">
              <q-pagination
                v-model="page"
                :max="10"
              ></q-pagination>
            </div>
          </div>
          <q-data-table
            :data="beers"
            :config="configs"
            :columns="columns">

            <template slot="col-image_url" scope="cell">
              <tooltip-button :url="cell.row.image_url"></tooltip-button>
            </template>
          </q-data-table>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  import tooltipButton from './tooltipButton.vue'
  export default {
    mounted () {
      this.getBeers()
    },
    data () {
      return {
        beers: [],
        page: 1,
        searchBeer: '',
        columns: [
          { label: 'Nome da Secretaria', field: 'name', width: '80px', sort: true }
        ],
        configs: {
          columnPicker: false,
          title: ''
        }
      }
    },
    watch: {
      page () {
        this.getBeers()
      }
    },
    computed: {
      url () {
        return `beers?page=${this.page}&per_page=10${this.search}`
      },
      search () {
        return this.searchBeer ? `&beer_name=${this.searchBeer}` : ''
      }
    },
    methods: {
      getBeers () {
        this.$http.punk
          .get(this.url)
          .then(response => { this.beers = response.data })
      }
    },
    components: {
      tooltipButton
    }
  }
</script>

<style scoped>
  .grid-filter{
    max-width: 200px
  }
  .grid-search{
    max-width: calc(100% - 400px)
  }
  .grid-pagination{
    max-width: 200px
  }
</style>
