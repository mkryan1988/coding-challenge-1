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
        // [ '3 2 N', 'FRRFLLFFRRFLL'],
        // [ '0 3 W', 'LLFFFLFLFL']
      ],
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
      console.log('pos:' + startingPos)
      for (let inst of instructions) {
        if (inst === 'L') {
          setTimeout(function() {
            newPos = self.rotateLeft(newPos)
          },500);
        } else if (inst === 'R') {
            setTimeout(function() {
              newPos = self.rotateRight(newPos)
            },500);
        } else if (inst === 'F') {
          setTimeout(function() {
            newPos = self.moveForward(newPos)
          },500);
        }
      }
      console.log('pos:' + newPos)
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
      return newPos.replace(/.$/, newdirection)
    },
    moveForward (newPos) {
      // if is past grid coordinates
      if (condition) {
        
      } else {
        return 
      }
      return newPos.replace(/.$/, newdirection)
    }
  },
  computed: {
    ifLost () {
      return 'LOST'
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
