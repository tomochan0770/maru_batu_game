<template>
  <div>
    <div class="row" v-for="y of 3" :key="y">
      <div class="cell" v-for="x of 3" :key="x" @click="clickEvent(y, x)">
        <v-btn width="100%" height="100%">{{ array[y - 1][x - 1] }}</v-btn>
      </div>
    </div>
    <v-dialog v-model="dialog" max-width="290">
    <v-card>
      <v-card-title class="headline">{{ victory }}チームの勝利</v-card-title>

      <v-card-text>
      勝利したあなたにはうんこをプレゼントします！
      </v-card-text>

      <v-card-actions>
        <v-spacer></v-spacer>
        <v-btn color="green darken-1" text @click="dialog = false">
          受け取る
        </v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>
  </div>
  
</template>

<script>
export default {
  data: function() {
    return {
      array: [
        [" ", " ", " "],
        [" ", " ", " "],
        [" ", " ", " "]
      ],
      count: 0,
      victory: " ",
      dialog : false
    };
  },
  methods: {
    chabgeTurn: function() {
      //どっちのターンかを決める。偶数→'○' 奇数→'×'
      this.count++;
      if (this.count % 2 === 1) {
        return "×";
      } else {
        return "○";
      }
    },

    //---------ここから下は勝敗の判定-----------
    isFilledArray: function(arr, val) {
      let i = 0;
      if (arr[i] === val && arr[i + 1] === val && arr[i + 2] === val) {
        return 1;
      } else {
        return 0;
      }
    },

    //横の場合
    horizon_line: function(arr, val) {
      for (let i = 0; i < 3; i++) {
        let array_horizon = [];
        for (let j = 0; j < 3; j++) {
          array_horizon.push(arr[i][j]);
        }
        if (this.isFilledArray(array_horizon, val) === 1) {
          return 1;
        }
      }
      return 0;
    },

    //縦の場合
    vertical_line: function(arr, val) {
      for (let i = 0; i < 3; i++) {
        let array_vertical = [];
        for (let j = 0; j < 3; j++) {
          array_vertical.push(arr[j][i]);
        }
        if (this.isFilledArray(array_vertical, val) === 1) {
          return 1;
        }
      }
      return 0;
    },

    //斜め(右下がり)の場合
    diagonal_line_right: function(arr, val) {
      let array_diagonal_right = [];
      for (let i = 0; i < 3; i++) {
        array_diagonal_right.push(arr[i][i]);
      }
      if (this.isFilledArray(array_diagonal_right, val) === 1) {
        return 1;
      }
      return 0;
    },

    //斜め(左下がり)の場合
    diagonal_line_left: function(arr, val) {
      let array_diagonal_left = [];
      let l = 2;
      for (let i = 0; i < 3; i++) {
        array_diagonal_left.push(arr[i][l]);
        l--;
      }
      if (this.isFilledArray(array_diagonal_left, val) === 1) {
        return 1;
      }
      return 0;
    },

    isBingo: function(arr, val) {
      let result = 0;
      //縦の場合
      result += this.horizon_line(arr, val);

      //横の場合
      result += this.vertical_line(arr, val);

      //斜め(右下がり)の場合
      result += this.diagonal_line_right(arr, val);

      //斜め(左下がり)の場合
      result += this.diagonal_line_left(arr, val);

      if (result > 0) {
        return true;
      } else {
        return false;
      }
    },

    //----------ここまで勝敗の判定--------------

    //=============  clickEvent  ================
    clickEvent: function(a, b) {
      //押した場所のデータと表記を変える
      const newRow = this.array[a - 1].slice(0);
      newRow[b - 1] = this.chabgeTurn();
      this.$set(this.array, a - 1, newRow);
      console.log("判定");

      //勝敗の判定
      if (this.count % 2 === 1) {
        if (this.isBingo(this.array, "×") === true) {
          console.log(this.count);
          this.victory = "×";
          this.dialog =true ;
        }
      } else {
        if (this.isBingo(this.array, "○") === true) {
          console.log(this.count);
          this.victory = "○";
          this.dialog =true ;
        }
      }
    }
  },
  computed: {}
};
</script>

<style>
.cell {
  border: 1px solid;
  width: 100px;
  height: 100px;
}
</style>
