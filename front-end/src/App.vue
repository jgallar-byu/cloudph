<template>
<div id="app">
  <div class="header">
    <router-link to="/">
      <div class="logo">
        <img src="/camera.png">
      </div>
    </router-link>
    <div class="title">
      <h1>Cloud's Photography</h1> 
    </div>
    <div class="right">
    <h3><a href="/AddEdit">Add Photography</a></h3>
    </div>
  </div>
  <div class="content">
    <router-view />
  </div>
  <div class="footer">
    <a href="https://github.com/jgallar-byu/cloudph">GitHub Code!</a>
  </div>
</div>
</template>

<style>

.title {
  max-width: 60%;
  margin: auto;
  padding: 10px;
}

.right {
  position: absolute;
  right: 0px;
  width: 200px;
  padding: 10px;
  color: white;
}

.right a:link, a:visited {
  background-color: #741212;
  color: white;
  padding: 3px 25px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
}

.right a:hover, a:active {
  background-color: black;
}

.right .router-link {
color: rgb(255, 255, 255);
}
.content {
  max-width: 60%;
  margin: auto;
  background: black;
  padding: 10px;

  color: white;
}

html {
  box-sizing: border-box;
}

body {
  font-family: 'Montserrat', sans-serif;
  font-size: 16px;
  background: moccasin;
  padding: 0px;
  margin: 0px;

  color: rgb(0, 0, 0)

}


.header {
  display: flex;
  padding: 10px 100px 0px 100px;
  background-color: #741212;
  color: #ffffff;
}

.title {
  margin-top: 5px;
}

.title h1 {
  font-size: 30px;
}

.content {
  padding: 20px 100px;
  min-height: 500px;
}


.footer {
  height: 60px;
  padding: 20px 100px 0px 100px;
  background: #741212;
  font-size: 12px;
}

.footer a {
  color: rgb(255, 255, 255);
}

h1 {
  font-size: 20px;
}

h2 {
  font-size: 14px;
}
</style>
