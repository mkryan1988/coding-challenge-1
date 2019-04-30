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
      var newPosStripped = newPos.replace(/\s+/g, '');
      var oldDirection = newPosStripped.charAt(2);
      var newdirection = ''
      if (oldDirection === 'N') {
        newDirection = 'W'
      } else if (oldDirection === 'E') {
        newDirection = 'N'
      } else if (oldDirection === 'S') {
        newDirection = 'E'
      } else if (oldDirection === 'W') {
        newDirection = 'S'
      }
      return newPos.replace(/.$/, newDirection)
    },
    rotateRight (newPos) {
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
