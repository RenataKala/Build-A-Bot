<template>
      <div class="content">
        <button class="add-to-cart" @click="addToCart()">Add to cart</button>
    <div class="top-row">
      <div class="top part" :style="headBorderStyle">
        <div class="robot-name">
            {{selectedRobot.head.title}}
            <span v-if="selectedRobot.head.onSale" class="sale">Sale!</span>
        </div>
        <img v-bind:src="selectedRobot.head.src" title="head"/>
        <button v-on:click="selectPrevHead()" class="prev-selector">&#9668;</button>
        <button v-on:click="selectNextHead()" class="next-selector">&#9658;</button>
      </div>
    </div>
    <div class="middle-row">
      <div class="left part">
        <img v-bind:src="selectedRobot.leftArm.src" title="left arm"/>
        <button v-on:click="selectPrevLeftArm()" class="prev-selector">&#9650;</button>
        <button v-on:click="selectNextLeftArm()" class="next-selector">&#9660;</button>
      </div>
      <div class="center part">
        <img v-bind:src="selectedRobot.torso.src" title="left arm"/>
        <button v-on:click="selectPrevTorso()" class="prev-selector">&#9668;</button>
        <button v-on:click="selectNextTorso()" class="next-selector">&#9658;</button>
      </div>
      <div class="right part">
        <img v-bind:src="selectedRobot.rightArm.src" title="left arm"/>
        <button v-on:click="selectPrevRightArm()" class="prev-selector">&#9650;</button>
        <button v-on:click="selectNextRightArm()" class="next-selector">&#9660;</button>
      </div>
    </div>
    <div class="bottom-row">
      <div class="bottom part">
        <img v-bind:src="selectedRobot.base.src" title="left arm"/>
        <button v-on:click="selectPrevBase()" class="prev-selector">&#9668;</button>
        <button v-on:click="selectNextBase()" class="next-selector">&#9658;</button>
      </div>
    </div>
    <div>
      <h1>Cart</h1>
      <table>
        <thead>
          <tr>
            <th>Robot</th>
            <th class="cost">Cost</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(robot,index) in cart" :key="index">
            <td>{{robot.head.title}}</td>
             <td class="cost">{{robot.cost}}</td>

          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
import aviableParts from '../data/parts';

function GetPreviousValidIndex(index, length){
  const deprecatedIndex = index -1 ;
  return deprecatedIndex < 0 ? length -1 : deprecatedIndex;
}

function GetNextValidIndex(index, length){
  const incrementedIndex = index -1 ;
  return incrementedIndex < 0 ? length -1 : incrementedIndex;
}

export default {
name:'RobotBuilder',
data(){
  return {
    aviableParts, 
    selectHeadIndex: 0,
    selectedLeftArmIndex :0,
    selectedRightArmIndex:0,
    selectedTorsoIndex:0,
    selectedBaseIndex:0,
    cart:[]
  
  };
},
computed:{
headBorderStyle(){
 return {
   border : this.selectedRobot.head.onSale ? 
   '3px solid red' : 
   '3px solid #aaa',};
  },
 selectedRobot() {
   return {
      head: aviableParts.heads[this.selectHeadIndex],
      leftArm :aviableParts.arms[this.selectedLeftArmIndex],
      rightArm : aviableParts.arms[this.selectedRightArmIndex],
      torso : aviableParts.torsos[this.selectedTorsoIndex],
      base : aviableParts.bases[this.selectedBaseIndex],
      };
  },
},
methods:{
  addToCart(){
    const robot = this.selectedRobot
    const cost = robot.head.cost + robot.leftArm.cost + robot.rightArm.cost 
    + robot.torso.cost + robot.base.cost;
    this.cart.push(Object.assign({ }, robot, { cost }))
  },
  selectNextHead(){
  this.selectHeadIndex =
  GetNextValidIndex(this.selectHeadIndex, aviableParts.heads.length)
  },
  selectPrevHead(){
    this.selectHeadIndex =
   GetPreviousValidIndex(this.selectHeadIndex, aviableParts.heads.length)
  }, 
  selectNextLeftArm(){
  this.selectedLeftArmIndex =
  GetNextValidIndex(this.selectedLeftArmIndex, aviableParts.arms.length)
  },
  selectNextRightArm(){
  this.selectedRightArmIndex =
  GetNextValidIndex(this.selectedRightArmIndex, aviableParts.arms.length)
  },
  selectNextTorso(){
  this.selectedTorsoIndex =
    GetNextValidIndex(this.selectedTorsoIndex, aviableParts.torsos.length)
  },
  selectNextBase(){
  this.selectedBaseIndex =
    GetNextValidIndex(this.selectedBaseIndex, aviableParts.bases.length)
  },
 selectPrevLeftArm(){
  this.selectedLeftArmIndex =
  GetPreviousValidIndex(this.selectedLeftArmIndex, aviableParts.arms.length)
  },
   selectPrevRightArm(){
  this.selectedRightArmIndex =
  GetPreviousValidIndex(this.selectedRightArmIndex, aviableParts.arms.length)
  },
  selectPrevTorso(){
  this.selectedTorsoIndex =
    GetPreviousValidIndex(this.selectedTorsoIndex, aviableParts.torsos.length)
  },
  selectPrevBase(){
  this.selectedBaseIndex =
    GetPreviousValidIndex(this.selectedBaseIndex, aviableParts.bases.length)
  },
  
  
},
};

</script>

<style lang="scss">
.part {
  position: relative;
  width:165px;
  height:165px;
  border: 3px solid #aaa;
} 
.part img {
  width:165px;
}
.top-row {
  display:flex;
  justify-content: space-around;
}
.middle-row {
  display:flex;
  justify-content: center;
}
.bottom-row {
  display:flex;
  justify-content: space-around;
  border-top: none;
}
.head {
  border-bottom: none;
}
.left {
  border-right: none;
}
.right {
  border-left: none;
}
.left img {
  transform: rotate(-90deg);
}
.right img {
  transform: rotate(90deg);
}
.bottom {
  border-top: none;
}
.prev-selector {
  position: absolute;
  z-index:1;
  top: -3px;
  left: -28px;
  width: 25px;
  height: 171px;
}
.next-selector {
  position: absolute;
  z-index:1;
  top: -3px;
  right: -28px;
  width: 25px;
  height: 171px;
}
.center .prev-selector, .center .next-selector {
  opacity:0.8;
}
.left .prev-selector {
  top: -28px;
  left: -3px;
  width: 144px;
  height: 25px;
}
.left .next-selector {
  top: auto;
  bottom: -28px;
  left: -3px;    
  width: 144px;
  height: 25px;
}
.right .prev-selector {
  top: -28px;
  left: 24px;  
  width: 144px;
  height: 25px;
}
.right .next-selector {
  top: auto;
  bottom: -28px;
  left: 24px;    
  width: 144px;
  height: 25px;
}
.right .next-selector {
  right: -3px;

}
.robot-name{
  position: absolute;
  top: -25px;
  text-align: center; 
  width: 100%;
}
.sale{
  color: red;
}
.content{
  position: relative;
}
.add-to-cart{
  position:absolute;
  right:20px;
  width:220px;
  padding: 3px;
  font-size:16px;
}
td,th{
  text-align: left;
  padding:5px;
  padding-right: 20px;
}
.cost{
  text-align:right;
}
</style>
