<template>
  <view class="container">
    <nb-container>
      <root>
        <user-list v-if="isReady"></user-list>
      </root>
    </nb-container>
  </view>
</template>

<script>
    import UserList from "./component/UserList"
    import {Root, VueNativeBase} from "native-base";
    import Vue from "vue-native-core";

    Vue.use(VueNativeBase);
    export default {
        data: function () {
            return {
                isReady: false
            }
        },
        created: function () {
            this.loadAppFonts();
        },
        methods: {
            loadAppFonts: async function () {
                try {
                    this.isReady = false;
                    await Expo.Font.loadAsync({
                        Roboto: require("native-base/Fonts/Roboto.ttf"),
                        Roboto_medium: require("native-base/Fonts/Roboto_medium.ttf"),
                        Ionicons: require("@expo/vector-icons/fonts/Ionicons.ttf")
                    });
                    this.isReady = true;
                } catch (error) {
                    console.log("Can't load fonts", error);
                    this.isReady = true;
                }
            },
        },
        components: {
            Root, UserList
        }

    }
</script>
<style>
  .container {
    justify-content: center;
    flex: 1;
  }
</style>
