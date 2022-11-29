---
layout: page
title: Projects
permalink: /projects/
description: Links to our projects. 
nav: true
nav_order: 1
---
<style>
  html {
  overflow-y: scroll;
}
.wrap
{
  margin:50px auto 0 auto;
  width:100%;
  display:flex;
  align-items:space-around;
  max-width:1200px;
}
.tile
{
  width:380px;
  height:380px;
  margin:10px;
  /* background-color:#95194B; */
  display:inline-block;
  background-size:cover;
  position:relative;
  cursor:pointer;
  transition: all 0.2s ease-out;
  box-shadow:rgba(0, 0, 0, 0.05) 0px 4px 20px 7px;
  overflow:hidden;
  color:white;
  font-family:'Roboto';
}
.tile img
{
  height:100%;
  width:100%;
  position:absolute;
  top:0;
  left:0;
  z-index:0;
  transition: all 0.2s ease-out;
}
.tile .text
{
/*   z-index:99; */
  position:absolute;
  padding:30px;
  height:calc(100% - 60px);
}
.tile h1
{
 
  font-weight:300;
  margin:0;
  text-shadow: 2px 2px 10px rgba(0,0,0,0.3);
}
.tile h2
{
  font-weight:100;
  margin:20px 0 0 0;
  font-style:italic;
   transform: translateX(200px);
}
.tile p
{
  font-weight:300;
  margin:20px 0 0 0;
  line-height: 25px;
}
.tile:hover
{
/* background-color:#95194B; */
box-shadow: 0px 35px 77px -17px rgba(0,0,0,0.64);
  transform:scale(1.02);
}
.tile:hover img
{
  opacity: 0.7;
}
.dots
{
  position:absolute;
  bottom:20px;
  right:30px;
  margin: 0 auto;
  width:30px;
  height:30px;
  color:currentColor;
  display:flex;
  flex-direction:column;
  align-items:center;
  justify-content:space-around;
  
}

.dots span
{
    width: 5px;
    height:5px;
    background-color: currentColor;
    border-radius: 50%;
    display:block;
  opacity:0;
  transition: transform 0.4s ease-out, opacity 0.5s ease;
  transform: translateY(30px);
 
}

.tile:hover span
{
  opacity:1;
  transform:translateY(0px);
}

.dots span:nth-child(1)
{
   transition-delay: 0.05s;
}
.dots span:nth-child(2)
{
   transition-delay: 0.1s;
}
.dots span:nth-child(3)
{
   transition-delay: 0.15s;
}


@media (max-width: 1000px) {
  .wrap {
   flex-direction: column;
    width:400px;
  }
}
</style>

<hr>
<div class="wrap">
<div class="tile">
<a href="{{ site.baseurl }}/projects/oct-vs-vf-accuracy-calculation"> 
  <!-- <img src='https://images.unsplash.com/photo-1464054313797-e27fb58e90a9?dpr=1&auto=format&crop=entropy&fit=crop&w=1500&h=996&q=80'/> -->
  <div class="text">
  <h1>VF and OCT Accuracy to Detect Glaucoma Worsening</h1>
  <p class="animate-text">Compare the accuracy of diagnosing glaucoma worsening over a 2-year period using OCT, VF, and both combined.</p>
<div class="dots">
    <span></span>
    <span></span>
    <span></span>
  </div>
  </div>
</a>
 </div>
</div>

