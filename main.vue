var i =1;
var executed = true;
 Vue.component('demo-grid', {
        template: '#grid-template',
        props: {
          data: Array,
          columns: Array,
          filterKey: String,
          searching: Number
        },
        data: function () {
          var sortOrders = {}
          this.columns.forEach(function (key) 
          {
            sortOrders[key] = 1;
          })
          return {
            sortKey: '',
            sortOrders: sortOrders,
            details: true,
            flag: true,
            quote: this.changeQuote(),
            show: true,
            shownresults:   []
          }
        },
        computed: 
        {  
          filteredData: function ()  {
            // this.loadResults()
;            // this should be executed only one at the begining
            return this.getResults();
          },
          foundResultsCount: function()  {
          var _foundResults = this.getResults();
          this.details = false;
          return _foundResults.length;
          },
          searchQuery: function(){
            return this.filterKey;
          },
          searchFilled: function(){
            if (this.filterKey==='') {return true}
              else return false;
          },
          randomQuote: function() {
             changeQuote();
          } 
        },
        filters: {
          capitalize: function (str)  {
            return str.charAt(0).toUpperCase() + str.slice(1)
          }
        },
        methods: 
        {
          sortBy: function (key) {
            this.sortKey = key
            this.sortOrders[key] = this.sortOrders[key] * -1
          },
          changeQuote: function() {
            var min=1;
            var max = this.data.length;
            var randomId = Math.floor(Math.random() * (max-min +1)) + min;
            var changedQuote = this.data[randomId];
            this.quote = changedQuote;
            return changedQuote;
          },
          getResults:  function()  {
              var sortKey = this.sortKey;
              var filterKey = this.filterKey && this.filterKey.toLowerCase()
              var order = this.sortOrders[sortKey] || 1
              var data = this.data
            if (filterKey) 
            {
              data = data.filter(function (row) {
              return Object.keys(row).some(function (key) {
              return String(row[key]).toLowerCase().indexOf(filterKey) > -1
                })
              })
            }
            if (sortKey) 
            {
              data = data.slice().sort(function (a, b) {
                a = a[sortKey]
                b = b[sortKey]
                return (a === b ? 0 : a > b ? 1 : -1) * order
               })
             }
            return data
          }
        }
      })
 
var app = new Vue({
  el: '#app',
  data: 
  {
    searchQuery: '',
    gridColumns: ['id','author', 'text'],
    //_quotes is the name of the JSON array from the data.js file 
    gridData: _quotes 
  }
})
