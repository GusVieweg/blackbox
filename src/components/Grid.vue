<template>
  <table>
    <tr v-for="rSquare in squares" :key="rSquare">
      <td v-for="cSquare in squares" :width="dynamicWidth" :key="cSquare" class="content" @click="highlightMe($event);handleDoubleClick($event)">
        <div :id="'r'+rSquare+'c'+cSquare" style="text-align:center;font-size:56px;"><br /></div>
      </td>
    </tr>
  </table>
</template>

<script>
import _ from 'lodash'

export default {
  name: "Grid",
  data: function () {
    return {
      mode: 'row',
      delay: 200,
      clicks: 0,
      timer: null,
      result: []
    }
  },
  props: {
    squares: Number
  },
  methods: {
    colorSquares: function(area, color, event) {
      if(area == 'grid') {
        // Get grid <table> and children
        let trs = _.find(event.path, ['localName', 'table']).children
      
        // Loop through and set color to white when not selected
        trs.forEach(tr => {
          tr.children.forEach(td => {
            td.children[0].style.backgroundColor = color
          })
        })
      }
      if(area == 'line') {
        if(this.mode == 'row') {
          // Traverse tr element and highlight all tds in the row
          let row = _.find(event.path, ['localName', 'tr'])
          row.children.forEach(td => {
            td.children[0].style.backgroundColor = '#dee8f2'
          })
        }
        if(this.mode == 'column') {
          // Get the column index and highlight all tds in that place in each row
          let trs = _.find(event.path, ['localName', 'table']).children
          let hIndex = event.srcElement.id.indexOf('c')
          let column = event.srcElement.id.substr(hIndex+1)
          trs.forEach(tr => {
            tr.children[column-1].children[0].style.backgroundColor = color
          })
        }
      }
      if(area == 'square') {
        event.srcElement.style.backgroundColor = 'rgba(25,85,165,.9)'
      }
    },
    highlightMe: function(e) {
      console.log(e)
      this.colorSquares('grid', 'white', e)
      this.colorSquares('line', '#dee8f2', e)
      this.colorSquares('square', 'rgba(25,85,165,.9)', e)
    },
    handleDoubleClick: function(event){
      this.clicks++ 
      if(this.clicks === 1) {
        var self = this
        this.timer = setTimeout(function() {
          self.result.push(event.type);
          self.clicks = 0
        }, this.delay);
      } else{
          clearTimeout(this.timer);
          console.log("double click!")
          this.switchMode(event)
          this.clicks = 0;
      }         
    },
    switchMode: function(event) {
      if(this.mode == 'row') {
        this.mode = 'column'
        console.log("mode is now column")
        this.highlightMe(event)
        return
      }
      if(this.mode == 'column') {
        this.mode = 'row'
        console.log("mode is now row")
        this.highlightMe(event)
        return
      }
    }
  },
  computed: {
    dynamicWidth: function() {
      return `${100/this.squares}%`
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
table {
  width: 25%;
  border-collapse: collapse;
}
tr:hover {
    background-color: #eee;
}
tr:hover td {
    background-color: transparent; /* or #000 */
}
/*td {
  position: relative;
}*/
td:after {
  content: '';
  display: block;
}
td .content {
  position: absolute;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
}
table, th, td {
  border: 1px solid black;
}

</style>
