---
title: "[Nomad Coders - React Native (3) - 스타일편]"
date:  2020-01-19
tags: [Javascript, ReactNative]
toc: true
---

## 1. Displaying Temperature

* prop-types설치

  ```
  npm install prop-types
  ```

  proptype이란 component에 들어온 값이 유효한 값인지 prop의 type을 미리 정의해놓는 것

* 코드

  ```javascript
  import React from "react";
  import PropTypes from "prop-types";
  import {View, Text, StyleSheet} from "react-native";
  
  export default function Weather({temp}){
      return (
          <View style={styles.container}>
              <Text>{temp}</Text>
          </View>
      );
  }
  
  Weather.propTypes = {
      temp:PropTypes.number.isRequired
  };
  
  const styles = StyleSheet.create({
      container: {
          flex: 1,
          justifyContent: "center",
          alignItems: "center"
      }
  });
  ```



## 2. Getting the Condition Names

코드

```javascript
Weather.propTypes = {
    temp: PropTypes.number.isRequired,
    condition: PropTypes.oneOf([
        "Thunderstorm",
        "Rain",
        "Drizzle",
        "Snow",
        "Atmosphere",
        "Clear",
        "Clouds",
        "Haze",
        "Mist",
        "Dust"
    ]).isRequired
};
```

api에서 정의한 날씨 목록을 condition으로 정의해줌.



## 3. Icons and Styling

* expo/vector-icons을 이용해서 아이콘 가져옴

  ```javascript
  import { MaterialCommunityIcons } from "@expo/vector-icons";
  //MaterialCommunityIcons이 우리가 원하는 아이콘을 많이 가지고 있음
  ```

* 요소 배치하기

  ```javascript
  <View style={styles.halfContainer}>
    <MaterialCommunityIcons
    size={96}
    name={weatherOptions[condition].iconName}
    color="white"
    />
    <Text style={styles.temp}>{temp}°</Text>
  </View>
  ```

* ```javascript
  const styles = StyleSheet.create({
    container: {
      flex: 1
    },
    temp: {
      fontSize: 42,
      color: "white"
    },
    halfContainer: {
      flex: 1,
      justifyContent: "center",
      alignItems: "center"
    }
  ```

## 4. Background Gradient

* gradient 하기 위해서 라이브러리 다운

  ```javascript
  expo install expo-linear-gradient
  ```

* 상태바 글씨 하얗게 바꾸기

  ```javascript
  import { View, Text, StyleSheet, StatusBar } from "react-native";
  ...
  
  <StatusBar barStyle="light-content" />
  ```

  디폴트는 dark-content이다.

## 5. 꿀팁!

visual studio code에서 여러 코드를 한번에 작성하거나 수정하고 싶다면,

Mac: opt+cmd+up 또는 opt+cmd+down 를 입력! 신기하다 ㅋㅋ

