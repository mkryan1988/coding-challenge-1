<template>
  <div class="hello">
    <button class="btn" :class="{'disabled' : loading || currentShip > 2}" @click="routeShip">Route Ship</button>
    <h4>Ships:</h4>
    <ol>
      <li v-for="(ship, index) in ships" :key="'ship-'+ index" >{{ship}}</li>
    </ol>
    <h4>Output:</h4>
    <ol>
      <li v-for="(value, index) in output" :key="'output-'+ index" >{{value}}</li>
    </ol>
    <h4>SOS coordinates:</h4>
    <ol>
      <li v-for="(value, index) in warnings" :key="'warning-'+ index" >{{value}}</li>
    </ol>
  </div>
</template>

<script>
export default {
  /* eslint-disable */
  name: 'oceanGrid',
  data () {
    return {
      route: [],
      ships: [
        [ '1 1 E', 'RFRFRFRF'],
        [ '3 2 N', 'FRRFLLFFRRFLL'],
        [ '0 3 W', 'LLFFFLFLFL']
      ],
      gridNE: '5 3',
      output: [],
      loading: false,
      warnings: [],
      currentShip: 0
    }
  },
  mounted () {
    // this.locateShips()
  },
  methods: {
    clearRouting () {
      var lastRoute = this.route[this.route.length - 1]
      this.output.push(lastRoute)
      this.route = []
      this.currentShip = this.currentShip + 1
      this.loading = false
    },
    routeShip () {
      var self = this
      self.loading = true
      var ship = self.ships[self.currentShip]
      const startingPos = ship[0]
      const instructions = Array.from(ship[1])
      var newPos = startingPos
      for (let [index, inst] of instructions.entries()) {
        if (inst === 'L') {
          setTimeout(function() {
            if (newPos) {
              newPos = self.rotateLeft(newPos)
            }
          },1000);
        } else if (inst === 'R') {
          setTimeout(function() {
            if (newPos) {
              newPos = self.rotateRight(newPos)
            }
          },1000);
        } else if (inst === 'F') {
          setTimeout(function() {
            if (newPos) {
              newPos = self.moveForward(newPos)
            }
          },1000);
        }
        if (index === instructions.length - 1) {
          setTimeout(function() {
            self.clearRouting()
          },1500);
        }
      };
    },
    rotateLeft (newPos) { 
      var newPosStripped = newPos.replace(/\s+/g, '');
      var oldDirection = newPosStripped.charAt(2);
      var newDirection = ''
      if (oldDirection === 'N') {
        newDirection = 'W'
      } else if (oldDirection === 'E') {
        newDirection = 'N'
      } else if (oldDirection === 'S') {
        newDirection = 'E'
      } else if (oldDirection === 'W') {
        newDirection = 'S'
      }
      this.route.push(newPos.replace(/.$/, newDirection))
      return newPos.replace(/.$/, newDirection)
    },
    rotateRight (newPos) {
      console.log(newPos)
      var newPosStripped = newPos.replace(/\s+/g, '');
      var oldDirection = newPosStripped.charAt(2);
      var newDirection = ''
      if (oldDirection === 'N') {
        newDirection = 'E'
      } else if (oldDirection === 'E') {
        newDirection = 'S'
      } else if (oldDirection === 'S') {
        newDirection = 'W'
      } else if (oldDirection === 'W') {
        newDirection = 'N'
      }
      this.route.push(newPos.replace(/.$/, newDirection))
      return newPos.replace(/.$/, newDirection)
    },
    moveForward (newPos) {
      var newPosStripped = newPos.replace(/\s+/g, '');
      var newX = Math.trunc(newPosStripped.charAt(0))
      var newY = Math.trunc(newPosStripped.charAt(1))
      var currentDirection = newPosStripped[newPosStripped.length -1];
      var coOrdinates = ''
      if (currentDirection === 'N') {
        newY = newY + 1
      } else if (currentDirection === 'E') {
        newX = newX + 1
      } else if (currentDirection === 'S') {
        newY = newY - 1
      } else if (currentDirection === 'W') {
        newX = newX - 1
      }
      // if past grid coordinates
      var maxX = Math.trunc(this.gridNE[0])
      var maxY = Math.trunc(this.gridNE[this.gridNE.length -1]) 
      if (newX < 0 || newX > maxX || newY < 0 || newY > maxY) {
        this.route.push(newPos + ' LOST')
        this.warnings.push(newPos)
        return false
      } else {
        // console.log(newX + ' ' + newY + ' ' + currentDirection);
        this.route.push(newX + ' ' + newY + ' ' + currentDirection)
        return newX + ' ' + newY + ' ' + currentDirection
        
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
.btn {
  text-decoration: none;
  color: #fff;
  background-color: #42b983;
  text-align: center;
  letter-spacing: .5px;
  -webkit-transition: background-color .2s ease-out;
  transition: background-color .2s ease-out;
  cursor: pointer;
  border: none;
  border-radius: 2px;
  display: inline-block;
  height: 36px;
  line-height: 36px;
  padding: 0 16px;
  text-transform: uppercase;
  vertical-align: middle;
  -webkit-tap-highlight-color: transparent;
}
.btn.disabled {
    pointer-events: none;
    background-color: #DFDFDF !important;
    -webkit-box-shadow: none;
    box-shadow: none;
    color: #9F9F9F !important;
    cursor: default;
}
</style>
