<template>
    <div className="wrapper">
        <h1>Погодное приложение</h1>
        <p>Узнать погоду в {{ city == '' ? 'вашем городе' : cityName }}</p>
        <input v-model="city" type="text" placeholder="введите город" />
        <button v-if="city != ''" @click="getCity()">Получить погоду</button>
        <button disabled v-else>Получить погоду</button>
        <p v-if="city.length < 2 && city.length != ''" className="error">
            {{ error }}
        </p>
        <div className="weatherBlock" v-if="info != null">
            <p className="weatherBlockP">{{ showTemp }}</p>
            <p className="weatherBlockP">{{ showFeelsLike }}</p>
            <p className="weatherBlockP">{{ showPressure }}</p>
            <p className="weatherBlockP">{{ showHumidity }}</p>
        </div>
    </div>
</template>

<script>
import axios from 'axios';
export default {
    data() {
        return { city: '', error: '', info: null };
    },
    computed: {
        cityName() {
            return '"' + this.city + '"';
        },
        showTemp() {
            return 'Температура: ' + this.info.temp;
        },
        showFeelsLike() {
            return 'Ощущается как: ' + this.info.feels_like;
        },
        showPressure() {
            return 'Давление: ' + this.info.pressure + ' мм.рт.ст.';
        },
        showHumidity() {
            return 'Влажность: ' + this.info.humidity + ' %';
        },
    },
    methods: {
        getCity() {
            if (this.city.trim().length < 2) {
                this.error = 'Название города слишком короткое';
                return false;
            }
            axios
                .get(
                    `https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=ab431d2d30fac6c4faf4c19b840f7939`
                )
                .then((res) => (this.info = res.data.main));
        },
    },
};
</script>

<style scoped>
.weatherBlock {
    width: 80%;
    margin: 0 auto;
    margin-top: 30px;
    padding: 20px;
    display: flex;
    flex-wrap: wrap;
    box-shadow: inset 0px 0px 10px 1px rgba(97, 97, 97, 1);
    justify-content: center;
    align-items: center;
}
.weatherBlockP {
    margin: 5px 0 !important;
    width: 50%;
    padding: 20px;
    transition: 1s;
}
.weatherBlockP:hover {
    --s: 10px; /* the spread-radius */
    position: relative;
    transform: translate(0);
    transform-style: preserve-3d;
}
.weatherBlockP::before {
    content: '';
    position: absolute;
    inset: calc(-1 * var(--s));
    clip-path: polygon(
        -100vmax -100vmax,
        100vmax -100vmax,
        100vmax 100vmax,
        -100vmax 100vmax,
        -100vmax -100vmax,
        calc(0px + var(--s)) calc(0px + var(--s)),
        calc(0px + var(--s)) calc(100% - var(--s)),
        calc(100% - var(--s)) calc(100% - var(--s)),
        calc(100% - var(--s)) calc(0px + var(--s)),
        calc(0px + var(--s)) calc(0px + var(--s))
    );
    transform: translate3d(0, 0, -1px);
    background: conic-gradient(
        from 90deg at 40% -25%,
        rgba(0, 61, 255, 1),
        rgba(0, 212, 255, 1),
        rgba(13, 134, 43, 1),
        rgba(13, 134, 43, 1),
        rgba(13, 134, 43, 1),
        rgba(6, 52, 14, 1)
    );
    filter: blur(10px);
}
.error {
    color: #d03939;
}
.wrapper {
    width: 900px;
    height: 500px;
    border-radius: 50px;
    background: #1b1c1b;
    padding: 20px;
    color: white;
    text-align: center;
}
.wrapper h1 {
    margin-top: 50px;
}
.wrapper p {
    margin-top: 20px;
}
.wrapper input {
    margin-top: 30px;
    background: transparent;
    border: 0;
    border-bottom: 2px solid #110813;
    color: #fcfcfc;
    font-size: 14px;
    padding: 5px 8px;
    outline: none;
}
.wrapper input:focus {
    border-bottom: 2px solid #6e2d7d;
}
.wrapper button {
    background: #e3bc4b;
    color: #fff;
    border-radius: 10px;
    border: 2px solid #b99935;
    padding: 10px 15px;
    margin-left: 20px;
    cursor: pointer;
    transition: transform 500ms ease;
}
.wrapper button:disabled {
    background: gray;
    cursor: not-allowed;
}
.wrapper button:hover {
    transform: scale(1.1) translateY(-5px);
}
</style>
