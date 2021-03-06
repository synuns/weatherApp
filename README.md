# ๐งweatherApp

<br />

Learning React Native by building a Foking Weather App

<br />

<p align="center"><img src="./img/run.gif"  width=50% height=auto /><p/>

[![](https://img.shields.io/badge/weatherApp-watch-white?logo=expo&labelColor=2c2c2c)](https://expo.io/@jwonder/projects/weatherApp)

<br />

## **โ ๋ชฉํ**

<br />

- weather API ํ์ฉ
- expo๋ฅผ ํตํด react native ๋น๋ํ๊ธฐ

<br />

## **๐ ์ฌ์ฉ๊ธฐ์ **

<br />

- javascript
- reactnative
- ๋ ์จ API (https://openweathermap.org/current)
- axios

<br />

## **๐น์ฃผ์๊ธฐ๋ฅ & ๐ป์ฝ๋ ๋ฆฌ๋ทฐ**

<br />

```
๐น์ฃผ์ ๊ธฐ๋ฅ
- GPS(์์น ์ ๋ณด) ๋ฐ๊ธฐ
- ์์น ์ ๋ณด๋ฅผ ํตํด ๋ ์จ ์ ๋ณด ์ ๊ณต
```

<br />

## **1. ๐GPS(์์น ์ ๋ณด) ๋ฐ๊ธฐ**

<br />

### **๐ป์ฝ๋ ๋ฆฌ๋ทฐ**

<br />

<p align="center">
  <img src="./img/gps.jpg" width=50% height=auto /> <img src="./img/error.jpg" width=50% height=auto">
<p/>

<br />

#### **๐App.js**

---

<br />

&nbsp; ์ฌ์ฉ์์ ์์น์ ๋ณด๋ฅผ ์ฒ๋ฆฌํ์ฌ ์ ๋ฌํฉ๋๋ค.

<br />

```js
getLocation = async () => {
  try {
    await Location.requestForegroundPermissionsAsync();
    const {
      coords: { latitude, longitude },
    } = await Location.getCurrentPositionAsync();
    this.getWeather(latitude, longitude);
  } catch (error) {
    Alert.alert("Can't find you.");
  }
};
```

<br />

> requestForegroyundPermissionAsync() ํจ์๋ก ์ฌ์ฉ์์ GPS์ ๊ทผ ๊ถํ์ ์์ฒญํฉ๋๋ค.
>
> getCurrentPositionAsync() ํจ์๋ก ์ฌ์ฉ์์ ์๋์ ๊ฒฝ๋ ์ ๋ณด๋ฅผ ์ป์ต๋๋ค.
>
> ์์น ์ ๋ณด ์ฒ๋ฆฌ ์์๋ฅผ ๋ณด์ฅํ๊ธฐ ์ํด ๋น๋๊ธฐ์  ์ฒ๋ฆฌ ํจํด(async await)์ผ๋ก ์์ฒญํฉ๋๋ค.
>
> ์๋์ ๊ฒฝ๋ ๋ณ์๋ฅผ ํตํด getWeather()ํจ์๋ฅผ ํธ์ถํฉ๋๋ค.

<br />

```js
getWeather = async (latitude, longitude) => {
  const {
    data: {
      main: { temp },
      weather,
      name,
    },
  } = await axios.get(
    `http://api.openweathermap.org/data/2.5/weather?lat=${latitude}&lon=${longitude}&appid=${API_KEY}&units=metric`
  );
  this.setState({
    isLoading: false,
    condition: weather[0].main,
    location: name,
    temp,
  });
};
```

<br />

> axios.get()์ ์ด์ฉํด weather api ๋ฐ์ดํฐ๋ฅผ ์ฒ๋ฆฌํฉ๋๋ค.
>
> weather api์์ ๋ ์จ, ์จ๋, ํ์ฌ ์์น์ ๋์ ์ด๋ฆ์ ๋ํ ์ ๋ณด๋ฅผ ๋ฐ์ต๋๋ค.
>
> api ์ ๋ณด๋ฅผ ๋ฐ์์ setState์ ์ ์ฅํ๊ณ  isLoading์ false๋ก ์๋ฐ์ดํธํฉ๋๋ค.

<br />

```js
  componentDidMount() {
    this.getLocation();
  }

  render() {
    const { isLoading, temp, condition, location } = this.state;
    return isLoading ? <Loading /> : <Weather temp={Math.round(temp)} condition={condition} location={location} />;
  }
```

<br />

> ์ปดํฌ๋ํธ๋ฅผ ์ด์ฉํด์ ์ํ๋ฅผ ์ ์ดํฉ๋๋ค.
>
> render๋ก ์์์์ loading ํ๋ฉด์ ๋ณด์ฌ์ฃผ๊ณ  componentDidMount๋ก getLocationํจ์๋ฅผ ํธ์ถํ ๋ค isLoading์ด false๋ก ๋ณํ๋ฉด weather.js๋ก ๋ ์จ ์ ๋ณด๋ฅผ ์ ๋ฌํฉ๋๋ค.

<br />

## **โ2. ์์น ์ ๋ณด๋ฅผ ํตํด ๋ ์จ ์ ๋ณด ์ ๊ณต**

<br />

### **๐ป์ฝ๋ ๋ฆฌ๋ทฐ**

<br />

<p align="center"><img src="./img/weather.jpg"  width=50% height=auto ><p/>

<br />

#### **๐weather.js**

---

<br />

&nbsp; api์์ ๋ฐ์ ๋ ์จ ์ ๋ณด๋ฅผ ํ๋ฉด์ ์ถ๋ ฅํฉ๋๋ค.

<br />

```js
Weather.propTypes = {
  temp: PropTypes.number.isRequired,
  condition: PropTypes.oneOf([
    'Thunderstorm',
    'Drizzle',
    'Rain',
    'Snow',
    'Atmosphere',
    'Mist',
    'Smoke',
    'Haze',
    'Dust',
    'Fog',
    'Sand',
    'Ash',
    'Squall',
    'Tornado',
    'Clear',
    'Clouds',
  ]).isRequired,
  location: PropTypes.string.isRequired,
};
```

<br />

> propType์ ์ ์ธํ์ฌ ๋ณ์ ์๋ ฅ๊ฐ์ ์ ํํฉ๋๋ค.

<br />

```js
const weatherOptions = {
    Thunderstorm: {
        iconName: "weather-lightning",
        gradient: ["#BA8B02", "#181818"],
        title: "Thunderstorm",
        subtitle: "When thunder roarsโก, go indoors. Find a safe, enclosed shelter."
    },
    Drizzle: {
        iconName: "weather-rainy",
        gradient: ["#355C7D", "#6C5B7B", "#C06C84"],
        title: "Drizzle",
        subtitle: "Take an umbrella, it's rainingโ"
    },

    ...(์ค๋ต)

    Clear: {
        iconName: "weather-sunny",
        gradient: ["#f8b500", "#fceabb"],
        title: "Clear",
        subtitle: "โEnjoy your dayโ"
    },
    Clouds: {
        iconName: "weather-cloudy",
        gradient: ["#a8c0ff", "#3f2b96"],
        title: "Clouds",
        subtitle: "โIt's a little cloudyโ"
    }
}
```

<br />

> ํ๋ฉด์ ์ถ๋ ฅํ  ์ ๋ณด๋ค์ object์ ์ ์ฅํ์ต๋๋ค.
>
> weather api์์ ์ ๊ณตํ๋ ๋ ์จ ๋ฐ์ดํฐ์ ๋ฐ๋ผ ๋ค๋ฅธ ์ ๋ณด๋ฅผ ์ ๊ณตํฉ๋๋ค.

<br />

```js
export default function Weather({ temp, condition, location }) {
  return (
    <LinearGradient
      colors={weatherOptions[condition].gradient}
      style={styles.container}
    >
      <StatusBar barStyle="light-content" />
      <View style={styles.header}>
        <FontAwesome name="location-arrow" size={28} color="white" />
        <Text style={styles.location}>{location}</Text>
      </View>
      <View style={styles.halfContainer}>
        <MaterialCommunityIcons
          name={weatherOptions[condition].iconName}
          size={96}
          color="white"
        />
        <Text style={styles.temp}>{temp}โ</Text>
      </View>
      <View style={{ ...styles.halfContainer, ...styles.textContainer }}>
        <Text style={styles.title}>{weatherOptions[condition].title}</Text>
        <Text style={styles.subtitle}>
          {weatherOptions[condition].subtitle}
        </Text>
      </View>
    </LinearGradient>
  );
}
```

<br />

> ์ฒ๋ฆฌํ ๋ ์จ ์ ๋ณด๋ฅผ ํตํด ํ๋ฉด์ ์ถ๋ ฅํฉ๋๋ค.

<br />

## **๐๋ง๋ฌด๋ฆฌ ์๊ฐ**

<br />

> react native์ ๋ํด์ ๊ณต๋ถํ  ์ ์๋ ๊ธฐํ์๋ค.
> style์ css์ ๋ค๋ฅด๊ฒ ์ฒ๋ฆฌํ๋ ๋ฐฉ์์ด ์ต์ํ์ง ์์๋ค.
> expo๋ฅผ ํตํด์ ์ฝ๊ฒ ์์ชฝ์ ์ด์์ฒด์ ์ ๋ํ ์ดํ๋ฆฌ์ผ์ด์์ ๋น๋ํ๊ณ  ๊ด๋ฆฌํ  ์ ์๋ค๋ ์ ์ด ์ ์ ํ๋ค.
>
> API ๋ฌธ์๋ฅผ ํ์ธํ์ฌ ์ํ๋ ์ ๋ณด๋ฅผ ์ฒ๋ฆฌํ  ์ ์๊ฒ ๋์๊ณ  ์ปดํฌ๋ํธ ์๋ช์ฃผ๊ธฐ์ ๋น๋๊ธฐ์ ์ธ ์ฒ๋ฆฌ ๋ฐฉ๋ฒ์ ํ์ตํ  ์ ์์๋ค.
>
> expo์ ๋์์ ๋ฐ์ ํ๋ก์ ํธ์ด์ง๋ง expo๋ฅผ ์ฌ์ฉํ๊ธฐ์ํด ์๊ฐ๋ ๋ง์ด ๋ค์๋ค.
> expo๋ฅผ ์ฌ์ฉํ์ง ์๊ณ  react native ํ๋ก์ ํธ๋ฅผ ํ๊ณ ์ถ๋ค.
> ๋ค์ ๋ฒ์๋ ์ ๋๋ก ์ฑ ๋น๋์์ ๋ฐฐํฌ๊น์ง ํด๋ณด๊ณ ์ถ๋ค.
>
> THANKS TO NOMADCODER, BAEGOFDAโบ

<br />
