<template>
  <div>
    <canvas
      ref="game"
      width="640"
      height="480"
      style="border: 1px solid black"
    ></canvas>
    <p>
      <button style="margin-right: 5px" v-on:click="move('right')">Right</button>
      <button style="margin-right: 5px" v-on:click="move('left')">Left</button>
      <button style="margin-right: 5px" v-on:click="move('up')">Up</button>
      <button style="margin-right: 5px" v-on:click="move('down')">Down</button>
    </p>
  </div>
</template>

<script>
import io from "socket.io-client";

export default {
  name: "Block",
  data() {
    return {
      socket: {},
      context: {},
      position: {
        x: 0,
        y: 0,
      },
    };
  },
  created() {
    this.socket = io("http://localhost:3000");
  },
  mounted() {
    this.context = this.$refs.game.getContext("2d");
    console.log(this.context)
    this.socket.on("position", (data) => {
      console.log('listening')
      this.position = data;
      console.log(this.context, 'inside arrow function')
      this.context.clearRect(
        0,
        0,
        this.$refs.game.width,
        this.$refs.game.height
      );
      this.context.fillRect(this.position.x, this.position.y, 20, 20);
    });
  },
  methods: {
    move(direction) {
      this.socket.emit("move", direction);
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style></style>
