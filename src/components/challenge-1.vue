<template>
  <div class="hello">
  </div>
</template>

<script>
export default {
  /* eslint-disable */
  name: 'oceanGrid',
  data () {
    return {
      ships: [
        [ '1 1 E', 'RFRFRFRF']
        // [ '3 2 N', 'FRRFLLFFRRFLL']
        // [ '0 3 W', 'LLFFFLFLFL']
      ],
      route: [],
      gridNE: '5 3',
      output: [],
    }
  },
  mounted () {
    this.locateShips()
  },
  methods: {
    locateShips () {
      for (var ship of this.ships) {
        this.routeShip(ship)
      }
    },
    routeShip(ship) {
      var self = this
      const startingPos = ship[0]
      const instructions = ship[1]
      var newPos = startingPos
      for (let inst of instructions) {
        if (inst === 'L') {
          setTimeout(function() {
            newPos = self.rotateLeft(newPos)
          },1000);
        } else if (inst === 'R') {
          setTimeout(function() {
            newPos = self.rotateRight(newPos)
          },1000);
        } else if (inst === 'F') {
          setTimeout(function() {
            newPos = self.moveForward(newPos)
          },1000);
        }
      }
    },
    rotateLeft (newPos) { 
      var oldDirection = newPos[newPos.length -1];
      var newdirection = ''
      if (oldDirection === 'N') {
        newdirection = 'W'
      } else if (oldDirection === 'E') {
        newdirection = 'N'
      } else if (oldDirection === 'S') {
        newdirection = 'E'
      } else if (oldDirection === 'W') {
        newdirection = 'S'
      }
      return newPos.replace(/.$/, newdirection)
    },
    rotateRight (newPos) {
      var oldDirection = newPos[newPos.length -1];
      var newdirection = ''
      if (oldDirection === 'N') {
        newdirection = 'E'
      } else if (oldDirection === 'E') {
        newdirection = 'S'
      } else if (oldDirection === 'S') {
        newdirection = 'W'
      } else if (oldDirection === 'W') {
        newdirection = 'N'
      }
      this.route.push(newPos.replace(/.$/, newdirection))
      return newPos.replace(/.$/, newdirection)
    },
    moveForward (newPos) {
      var newPosStripped = newPos.replace(/\s+/g, '');
      var newX = Math.trunc(newPosStripped.charAt(0))
      var newY = Math.trunc(newPosStripped.charAt(1))
      var oldDirection = newPosStripped[newPosStripped.length -1];
      var coOrdinates = ''
      if (oldDirection === 'N') {
        newY = newY + 1
      } else if (oldDirection === 'E') {
        newX = newX + 1
      } else if (oldDirection === 'S') {
        newY = newY - 1
      } else if (oldDirection === 'W') {
        newX = newX - 1
      }
      // if past grid coordinates
      var maxX = Math.trunc(this.gridNE[0])
      var maxY = Math.trunc(this.gridNE[this.gridNE.length -1]) 
      if ((newX >= 0 && newX <= maxX) || (newY > 0 && newY <= maxY)) {
        // console.log(newX + ' ' + newY + ' ' + oldDirection);
        this.route.push(newX + ' ' + newY + ' ' + oldDirection)
        return newX + ' ' + newY + ' ' + oldDirection
      } else {
        this.route.push(newPos + 'LOST')
        return newPos + 'LOST'
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
</style>
