<template>
<div class="corona-results-card mt-5">
    <div class="row" v-if="showFilter">
        <div class="col-sm-12">
              <div class="form-group">
                <label for="filter" class="filter-heading">Filter</label>
                <div class="input-group mb-3">
                    <input type="text" class="form-control" placeholder="" aria-label="Example text with button addon" aria-describedby="button-addon1">
                    <div class="input-group-prepend">
                        <button class="btn btn-outline-secondary" type="button" id="button-addon1">Filter</button>
                    </div>
                </div>
                <small id="filterHelp" class="form-text text-muted" v-if="isLoading">Loading...</small>
            </div>
        </div>
    </div>
    <div class="row">
        <div class='col-md-4 mb-4' v-for="result in results" :key="result.country_name">
            <div class="card">
                <div class="card-header">
                    {{result.country_name}}
                </div>
                <div class="card-body">
                    <!-- <h5 class="card-title">Special title treatment</h5> -->
                    <!-- <p class="card-text">With supporting text below as a natural lead-in to additional content.</p> -->
                    <ul>
                        <li><strong>Active cases: </strong> {{result.active_cases}}</li>
                        <li><strong>Deaths: </strong> {{result.deaths}}</li>
                        <li><strong>New cases: </strong> {{result.new_cases}}</li>
                        <li><strong>New Deaths: </strong> {{result.new_deaths}}</li>
                    </ul>
                </div>
            </div>
        </div>
    </div>
</div>

</template>

<script>
import axios from 'axios'
export default {
  name: 'CoronaResultsCard',
  data: function () {
    return {
      isLoading: false,
      results: {}
    }
  },
  props: ['showFilter'],
  methods: {
    getWorldCoronaData: function () {
      this.isLoading = true
      axios.get('https://coronavirus-monitor.p.rapidapi.com/coronavirus/cases_by_country.php', {
        headers: {
          'x-rapidapi-host': 'coronavirus-monitor.p.rapidapi.com',
          'x-rapidapi-key': '8e840460e2mshfcdbd3d224f6009p1a0482jsnd6d94842aed3'
        }
      })
        .then(response => {
          this.results = response.data.countries_stat
        })
        .catch(e => {
          this.errors.push(e)
        })
      this.isLoading = false
    }
  },
  beforeMount () {
    this.getWorldCoronaData()
  }
}
</script>
<style scoped>
    .filter-heading{
        font-weight: bold;
        display: flex;
        justify-content: left;
    }
    .card-header{
        font-weight: bold;
    }
    ul{
        list-style: none;
        text-align: left;
        padding: 0px;
    }
</style>
