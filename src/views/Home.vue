<template>
  <div class="home-container">
    <!-- Primary Info --------------------------------------------->
    <div class="home-primary-grid home-primary-container">
      <!-- Speed Info -->
      <div class="home-primary-grid-item">
        <SpeedGauge :value="speed" :maxValue="200" />
      </div>

      <!-- Torque Info -->
      <div class="home-primary-grid-item">
        <RPMGauge :value="torque" :maxValue="8000" />
      </div>

      <!-- Gear Info -->
      <div class="home-primary-grid-item">
        <span class="home-primary-info gear-info">
          {{ GearText }}
        </span>
        <img src="../assets/images/icons/tractor.png" class="gear-icon" alt="?"/>
      </div>
    </div>

    <!-- Secondary Info ------------------------------------------->
    <div class="home-secondary-grid home-secondary-container">
      <!-- Engine Temperature Info -->
      <div class="home-secondary-grid-item">
        <!-- <BasicInfo icon="temp-half.png" description="Oil Temperature" :value="oilTemp" unit="°C" class="home-basic-info"/> -->
        <TemperatureBar :value="oilTemp" :maxValue="120" :minValue="0" description="Oil Temperature" />
      </div>

      <!-- Engine Power Info -->
      <div class="home-secondary-grid-item">
        <!-- <BasicInfo icon="engine-battery.png" description="Oil Pressure" :value="oilPressure" unit="PSI" class="home-basic-info"/> -->
        <TemperatureBar :value="oilPressure" :maxValue="120" :minValue="0" description="Oil Pressure" />
      </div>

      <!-- Drive Hours Info -->
      <div class="home-secondary-grid-item">
        <BasicInfo icon="hourglass.png" description="Drive Hours" :value="driveHours" unit="Hr" class="home-basic-info"/>
      </div>
    </div>
  </div>
</template>

<script>
import BasicInfo from "@/components/BasicInfo.vue";
import RPMGauge from "@/components/RPMGauge.vue";
import SpeedGauge from "@/components/SpeedGauge.vue";
import TemperatureBar from "@/components/TemperatureBar.vue";

export default {
  name: "UmattHome",
  components: { BasicInfo, RPMGauge, SpeedGauge, TemperatureBar },
  computed: {
    GearText() {
      if (this.$store.state.gear === 0) {
        return "P"
      } else if (this.$store.state.gear === 1) {
        return "R"
      } else if (this.$store.state.gear === 2) {
        return "N"
      } else if (this.$store.state.gear === 3) {
        return "D"
      } else {
        return ""
      }
    },
    speed() {
      return this.$store.state.speed;
    },
    torque() {
      return this.$store.state.torque;
    },
    oilTemp() {
      return this.$store.state.oilTemp;
    },
    oilPressure() {
      return this.$store.state.oilPressure;
    },
    gslPosition() {
      return this.$store.state.gslPosition;
    },
    driveHours() {
      return this.$store.state.driveHours;
    },
    systemTime() {
      return this.$store.state.systemTime;
    }
  }
}
</script>

<style lang="scss">
@import "@/assets/styling.scss";

/* container layouts */
.home-container {
  // color
  background-color: $homeBackground;
  // border styling
  border: 0px solid $infoBorder;
  border-radius: 5px;
  // margin & padding
  margin: 10px;
  padding: 10px;
}
.home-primary-container {
  // color
  background-color: $homeBackground;
  // border styling
  border: 0px solid $homeBorder;
  border-radius: 5px;
  // size
  height: 38%;
  // margin & padding
  margin: 10px;
  padding: 10px;
}
.home-secondary-container {
  // color
  background-color: $homeBackground;
  // border styling
  //border: 3px solid $infoBorder;
  //border-radius: 5px;
  // margin & padding
  margin: 10px;
  padding: 10px;
}

/* grid layout */
.home-primary-grid {
  display: grid;
  grid-template-columns: 3fr 3fr 2fr;
  gap: 5px 5px;
}
.home-secondary-grid {
  display: grid;
  grid-template-rows: 1fr 1fr 1fr;
  gap: 5px 5px;
}
.home-primary-grid-item {
  // align content horizontally & vertically to the center
  display: flex;
  justify-content: center;
  align-items: center;
}
.home-secondary-grid-item {

}

/* info styling */
.speed-info {
  // text styling
  font-size: 8em;
  font-weight: bold;
}
.speed-unit {
  // text styling
  font-size: 32px;
  font-weight: bold;
  // margin & padding
  margin-left: 10px;
}
.torque-info {
  // text styling
  font-size: 6em;
  font-weight: bold;
}
.torque-unit {
  // text styling
  font-size: 32px;
  font-weight: bold;
  // margin & padding
  margin-left: 10px;
}
.gear-info {
  // text styling
  font-size: 9em;
  font-weight: bold;
}
.gear-icon {
  // image sizing
  max-height: 70px;
  max-width: 70px;
  // margin & padding
  margin-left: 15px;
}
.home-basic-info {

}
</style>