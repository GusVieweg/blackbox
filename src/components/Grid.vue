<template>
  <table>
    <tr v-for="rSquare in squares" :key="rSquare">
      <td
        v-for="cSquare in squares"
        :width="dynamicWidth"
        :key="cSquare"
        class="content"
        @click="handleSquare($event)"
      >
        <div
          :ref="'r' + (rSquare - 1) + 'c' + (cSquare - 1)"
          :id="'r' + (rSquare - 1) + 'c' + (cSquare - 1)"
          style="text-align:center;font-size:56px;"
        >
          <br />
        </div>
      </td>
    </tr>
  </table>
</template>

<script>
// import _ from "lodash";

export default {
  name: "Grid",
  data: function() {
    return {
      mode: "row",
      delay: 200,
      clicks: 0,
      timer: null,
      result: [],
      grid: [],
      currentSquare: "r0c0"
    };
  },
  props: {
    squares: Number
  },
  beforeMount() {
    this.buildGrid();
  },
  mounted() {
    window.addEventListener("keydown", e => {
      if (e.which !== 0) {
        let charCode = e.keyCode;
        //  Check if in the alphabet
        if (
          (charCode > 64 && charCode < 91) ||
          (charCode > 96 && charCode < 123)
        ) {
          this.insertLetterIntoGrid(String.fromCharCode(e.keyCode));
          this.progressSquare("forward");
        }
      }
      if (e.code == "Backspace") {
        this.deleteLetterFromGrid();
        this.progressSquare("backward");
      }
    });
  },
  methods: {
    handleSquare: function(event) {
      if (event.type == "click") {
        this.currentSquare = event.srcElement.id;
        this.handleDoubleClick();
      }
      this.highlightMe();
    },
    deleteLetterFromGrid() {
      if (this.currentSquare) {
        this.$refs[this.currentSquare][0].innerHTML = "<br />";
        this.grid[this.currentRow][this.currentColumn] = "?";
      }
    },
    insertLetterIntoGrid(letter) {
      if (this.currentSquare) {
        this.$refs[this.currentSquare][0].innerHTML = letter;
        this.grid[this.currentRow][this.currentColumn] = letter;
      }
    },
    progressSquare(direction) {
      let exceeded = false;
      if (this.currentSquare) {
        // Error checking for grid-end conditions
        if (direction == "forward") {
          if (this.mode == "row") {
            exceeded = Number(this.currentColumn) + 1 >= this.squares;
            let newColumn = exceeded
              ? Number(this.currentColumn)
              : Number(this.currentColumn) + 1;
            this.currentSquare = "r" + this.currentRow + "c" + newColumn;
          }
          if (this.mode == "column") {
            exceeded = Number(this.currentRow) + 1 >= this.squares;
            let newRow = exceeded
              ? Number(this.currentRow)
              : Number(this.currentRow) + 1;
            this.currentSquare = "r" + newRow + "c" + this.currentColumn;
          }
        } else {
          if (this.mode == "row") {
            exceeded = Number(this.currentColumn) - 1 < 0;
            let newColumn = exceeded ? 0 : Number(this.currentColumn) - 1;
            this.currentSquare = "r" + this.currentRow + "c" + newColumn;
          }
          if (this.mode == "column") {
            exceeded = Number(this.currentRow) - 1 < 0;
            let newRow = exceeded ? 0 : Number(this.currentRow) - 1;
            this.currentSquare = "r" + newRow + "c" + this.currentColumn;
          }
        }
        // Skip over existing letters
        if (
          direction == "forward" &&
          this.currentContents != "?" &&
          !exceeded
        ) {
          this.progressSquare("forward");
        }
        this.handleSquare("keypress");
      }
    },
    buildGrid: function() {
      let g = new Array(this.squares);
      for (var i = 0; i < g.length; i++) {
        g[i] = new Array(this.squares).fill("?");
      }
      this.grid = g;
    },
    getFromCurrentSquare(element) {
      if (element == "grid") {
        console.log(this.currentSquare);
        return this.$refs[this.currentSquare][0].parentElement.parentElement
          .parentElement;
      }
      if (element == "row") {
        return this.$refs[this.currentSquare][0].parentElement.parentElement;
      }
      if (element == "square") {
        return this.$refs[this.currentSquare][0];
      }
    },
    colorSquares: function(area, color) {
      if (area == "grid") {
        // Get grid <table> and children
        let trs = this.getFromCurrentSquare("grid").children;

        // Loop through and set color to white when not selected
        trs.forEach(tr => {
          tr.children.forEach(td => {
            td.children[0].style.backgroundColor = color;
          });
        });
      }
      if (area == "line") {
        if (this.mode == "row") {
          // Traverse tr element and highlight all tds in the row
          let row = this.getFromCurrentSquare("row");
          row.children.forEach(td => {
            td.children[0].style.backgroundColor = "#dee8f2";
          });
        }
        if (this.mode == "column") {
          // Get the column index and highlight all tds in that place in each row
          let trs = this.getFromCurrentSquare("grid").children;
          let column = this.currentColumn;
          trs.forEach(tr => {
            tr.children[column].children[0].style.backgroundColor = color;
          });
        }
      }
      if (area == "square") {
        let square = this.getFromCurrentSquare("square");
        square.style.backgroundColor = color;
      }
    },
    highlightMe: function() {
      this.colorSquares("grid", "white");
      this.colorSquares("line", "#dee8f2");
      this.colorSquares("square", "rgba(25,85,165,.9)");
    },
    handleDoubleClick: function() {
      this.clicks++;
      if (this.clicks === 1) {
        var self = this;
        this.timer = setTimeout(function() {
          self.result.push("click");
          self.clicks = 0;
        }, this.delay);
      } else {
        clearTimeout(this.timer);
        console.log("double click!");
        this.switchMode();
        this.clicks = 0;
      }
    },
    switchMode: function() {
      if (this.mode == "row") {
        this.mode = "column";
        this.highlightMe();
        return;
      }
      if (this.mode == "column") {
        this.mode = "row";
        this.highlightMe();
        return;
      }
    }
  },
  computed: {
    dynamicWidth: function() {
      return `${100 / this.squares}%`;
    },
    currentRow: function() {
      if (this.currentSquare) {
        return this.currentSquare.substring(
          this.currentSquare.lastIndexOf("r") + 1,
          this.currentSquare.lastIndexOf("c")
        );
      }
      return 0;
    },
    currentColumn: function() {
      if (this.currentSquare) {
        return this.currentSquare.substring(
          this.currentSquare.lastIndexOf("c") + 1
        );
      }
      return 0;
    },
    currentContents: function() {
      return this.grid[this.currentRow][this.currentColumn];
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
  content: "";
  display: block;
}
td .content {
  position: absolute;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
}
table,
th,
td {
  border: 1px solid black;
}
</style>
