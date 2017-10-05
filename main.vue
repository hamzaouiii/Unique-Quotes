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
            show: true
          }
        },
        computed: 
        {  
          filteredData: function ()  {
            // this.slowdown();
            return this.getResult() ;
          },
          
          count: function()  {
          if ( this.filteredData.length == this.data.length )    {  this.details = true;   return "all";}
           else  {this.details = false;return this.filteredData.length;}
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
          }
          ,
          changeQuote: function() {
            var min=1;
            var max = this.data.length;
            var randomId = Math.floor(Math.random() * (max-min +1)) + min;
            var changedQuote = this.data[randomId];
            this.quote = changedQuote;
            return changedQuote;
          },
          getResult:  function()  {
              var sortKey = this.sortKey;
              var filterKey = this.filterKey && this.filterKey.toLowerCase()
              var order = this.sortOrders[sortKey] || 1
              var data = this.data
            console.log(filterKey);
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
 
var demo = new Vue({
  el: '#demo',
  data: 
  {
    searchQuery: 'camus',
    gridColumns: ['id','author', 'text'],
    //_quotes is the name of the JSON array from the data.js file 
    gridData: _quotes 
  }
})