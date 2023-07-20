<template>
  <div class="navbar" :class="{ 'navbar-hovered': showNavibar }" @mouseenter="showNavbar" @mouseleave="hideNavbar">
    <div class="navbar-items">
      <div class="navbar-item" @mouseenter="showVisualTransformations">
        <span class="navbar-icon"></span>
        Visual Transformation
        <div class="navbar-dropdown" v-show="showVisualTransformationsDropdown">
          <ul>
            <li>Satellite Map</li>
            <li>Normal Map</li>
          </ul>
        </div>
      </div>
      <div class="navbar-item">
        <span class="navbar-icon"></span>
        View Mode
        <div class="navbar-dropdown">
          <ul>
            <li><router-link class="globalFont" to="/sideview">Side View</router-link></li>
            <li><a class="globalFont" href="/verticalview">Vertical View</a></li>
            <li><router-link class="globalFont" to="/MainVisionmode">3D Mode</router-link></li>
          </ul>
        </div>
      </div>
      <div class="navbar-item">
        <span class="navbar-icon"></span>
        Picture-in-Picture
        <div class="navbar-dropdown">
          <ul>
            <li @click="selectPicture('Sun Eclipse')">Sun Eclipse</li>
            <li @click="selectPicture('Moon Eclipse')">Moon Eclipse</li>
          </ul>
        </div>
      </div>
    </div>
  </div>
</template>
  
<script>
export default class NavbarComponent {
  constructor() {
    this.showNavibar = false;
    this.showVisualTransformationsDropdown = false;
  }

  showNavbar() {
    this.showNavibar = true;
  }

  hideNavbar() {
    this.showNavibar = false;
  }

  showVisualTransformations() {
    this.showVisualTransformationsDropdown = true;
  }

  selectPicture(picture) {
    // Invoke your callback here with the selected picture
    console.log("Selected Picture:", picture);
  }
}
</script>
  
<style scoped>
@font-face {
  font-family: 'Montserrat';
  /* Font name to use later */
  src: url('../assets/fonts/Montserrat-Light.ttf') format('truetype');
  /* URL to the TTF file */
  /* Add other font properties here if needed */
}

.navbar {
  display: flex;
  justify-content: center;
  width: 100%;
  height: 60px;
  /* opacity: 0.5; */
  background: rgba(0, 0, 0, 0);
  border-radius: 15px;
  position: fixed;
  top: 14px;
  left: 0;
  z-index: 3;
  align-items: center;
}

.navbar-items {
  display: flex;
  justify-content: center;
  color: white;
  font-size: 21px;
  font-family: Montserrat;
  background: rgba(0, 0, 0, 0);
  font-weight: 400;
  /* opacity: 0; */
  /* z-index: 3; */
  word-wrap: break-word;
  align-items: center;

}

.navbar-item {
  margin-right: 10px;
  justify-content: center;
  position: relative;
  align-items: center;
  margin-left: 30px;
  margin-right: 30px;
  /* opacity: 0; */
}

.navbar:hover .navbar-items {
  top: auto;
  justify-content: center;
  left: 10;
  margin: 15px;
}

/* .navbar-item {
  display: inline-block;
  margin-right: 10px;
  position: relative;
} */

.navbar-icon {
  display: inline-block;
  margin-right: 5px;
  width: 25px;
  height: 25px;
  transform-origin: 0 0;
  background-image: url('../assets/home/right-arrow.png');
  z-index: 10;
  position: relative;
}

.navbar-dropdown {
  /* position: absolute;
  top: 20px;
  left: 0; */
  justify-content: center;
  display: none;
  z-index: 100;
}

.navbar-item:hover .navbar-dropdown {
  display: block;
  z-index: 100;
}

.navbar-dropdown ul {
  list-style-type: none;
  padding: 0;
  margin-top: 30px;
  position: relative;
}

.navbar-dropdown li {
  margin-top: 7px;
}

.navbar-dropdown ul {
  list-style-type: none;
  padding: 0;
}

.navbar-dropdown li {
  cursor: pointer;
  transition: font-size 0.5s;
}

.navbar-dropdown li:hover {
  font-size: 25px;
}

.globalFont:hover {
  font-size: 25px;
}

.globalFont {
  font-family: Montserrat;
  font-weight: 400;
  font-size: 22px;
  color: white;
  text-decoration: none;
  transition: font-size 0.5s;
}
</style>
  