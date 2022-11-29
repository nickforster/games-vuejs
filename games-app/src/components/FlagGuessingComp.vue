<template>
  <div id="game">
    <div id="img-container">
      <img :src="imageSource" alt="The flag to be guessed" height="400">
    </div>
    <div id="stats">
      <p>{{ answersTotal }} / 201 answered</p>
      <p>{{ answersCorrect }} correct</p>
      <p>{{ answersWrong }} wrong</p>
    </div>
    <div id="answer-buttons">
      <button v-for="(answer, index) in answers" :key="index" @click="answerClicked(index)">
        {{ answer }}
      </button>
    </div>
  </div>
  <button id="back-button"><a href="#">back</a></button>
  <button id="reset-button" @click="resetGame">reset</button>
</template>
<script>
function shuffleObject(obj) {
  let newObj = {};
  let keys = Object.keys(obj);
  keys.sort(() => {
    return Math.random() - 0.5;
  });
  keys.forEach((k) => {
    newObj[k] = obj[k];
  });
  return newObj;
}

export default {
  data() {
    return {
      countries: {
        ad: "Andorra",
        ae: "United Arab Emirates",
        af: "Afghanistan",
        ag: "Antigua and Barbuda",
        al: "Albania",
        am: "Armenia",
        ao: "Angola",
        aq: "Antarctica",
        ar: "Argentina",
        at: "Austria",
        au: "Australia",
        az: "Azerbaijan",
        ba: "Bosnia and Herzegovina",
        bb: "Barbados",
        bd: "Bangladesh",
        be: "Belgium",
        bf: "Burkina Faso",
        bg: "Bulgaria",
        bh: "Bahrain",
        bi: "Burundi",
        bj: "Benin",
        bn: "Brunei",
        bo: "Bolivia",
        br: "Brazil",
        bs: "Bahamas",
        bt: "Bhutan",
        bw: "Botswana",
        by: "Belarus",
        bz: "Belize",
        ca: "Canada",
        cd: "Congo (Dem. Rep.)",
        cf: "Central African Republic",
        cg: "Congo",
        ch: "Switzerland",
        ci: "Ivory Coast",
        ck: "Cook Islands",
        cl: "Chile",
        cm: "Cameroon",
        cn: "China",
        co: "Colombia",
        cr: "Costa Rica",
        cu: "Cuba",
        cv: "Cape Verde",
        cy: "Cyprus",
        cz: "Czech Republic",
        de: "Germany",
        dj: "Djibouti",
        dk: "Denmark",
        dm: "Dominica",
        do: "Dominican Republic",
        dz: "Algeria",
        ec: "Ecuador",
        ee: "Estonia",
        eg: "Egypt",
        er: "Eritrea",
        es: "Spain",
        et: "Ethiopia",
        fi: "Finland",
        fj: "Fiji",
        fk: "Falkland Islands",
        fm: "Micronesia",
        fr: "France",
        ga: "Gabon",
        gb: "United Kingdom",
        gh: "Ghana",
        gl: "Greenland",
        gm: "Gambia",
        gn: "Guinea",
        gq: "Equatorial Guinea",
        gr: "Greece",
        gt: "Guatemala",
        gw: "Guinea-Bissau",
        gy: "Guyana",
        hk: "Hong Kong",
        hn: "Honduras",
        hr: "Croatia",
        ht: "Haiti",
        hu: "Hungary",
        id: "Indonesia",
        ie: "Ireland",
        il: "Israel",
        in: "India",
        iq: "Iraq",
        ir: "Iran",
        is: "Iceland",
        it: "Italy",
        jm: "Jamaica",
        jo: "Jordan",
        jp: "Japan",
        ke: "Kenya",
        kg: "Kyrgyzstan",
        kh: "Cambodia",
        ki: "Kiribati",
        km: "Comoros",
        kn: "Saint Kitts and Nevis",
        kp: "North Korea",
        kr: "South Korea",
        kw: "Kuwait",
        kz: "Kazakhstan",
        la: "Laos",
        lb: "Lebanon",
        lc: "Saint Lucia",
        li: "Liechtenstein",
        lk: "Sri Lanka",
        lr: "Liberia",
        ls: "Lesotho",
        lt: "Lithuania",
        lu: "Luxembourg",
        lv: "Latvia",
        ly: "Libya",
        ma: "Morocco",
        mc: "Monaco",
        md: "Moldova",
        me: "Montenegro",
        mg: "Madagascar",
        mh: "Marshall Islands",
        mk: "North Macedonia",
        ml: "Mali",
        mm: "Myanmar",
        mn: "Mongolia",
        mo: "Macau",
        mr: "Mauritania",
        mt: "Malta",
        mu: "Mauritius",
        mv: "Maldives",
        mw: "Malawi",
        mx: "Mexico",
        my: "Malaysia",
        mz: "Mozambique",
        na: "Namibia",
        ne: "Niger",
        ng: "Nigeria",
        ni: "Nicaragua",
        nl: "Netherlands",
        no: "Norway",
        np: "Nepal",
        nr: "Nauru",
        nz: "New Zealand",
        om: "Oman",
        pa: "Panama",
        pe: "Peru",
        pg: "Papua New Guinea",
        ph: "Philippines",
        pk: "Pakistan",
        pl: "Poland",
        pr: "Puerto Rico",
        ps: "Palestine",
        pt: "Portugal",
        py: "Paraguay",
        qa: "Qatar",
        ro: "Romania",
        rs: "Serbia",
        ru: "Russia",
        rw: "Rwanda",
        sa: "Saudi Arabia",
        sb: "Solomon Islands",
        sc: "Seychelles",
        sd: "Sudan",
        se: "Sweden",
        sg: "Singapore",
        si: "Slovenia",
        sk: "Slovakia",
        sl: "Sierra Leone",
        sm: "San Marino",
        sn: "Senegal",
        so: "Somalia",
        sr: "Suriname",
        ss: "South Sudan",
        st: "São Tomé und Príncipe",
        sv: "El Salvador",
        sy: "Syria",
        sz: "Swaziland",
        td: "Chad",
        tg: "Togo",
        th: "Thailand",
        tj: "Tajikistan",
        tl: "East Timor",
        tm: "Turkmenistan",
        tn: "Tunisia",
        to: "Tonga",
        tr: "Turkey",
        tt: "Trinidad and Tobago",
        tv: "Tuvalu",
        tw: "Taiwan",
        tz: "Tanzania",
        ua: "Ukraine",
        ug: "Uganda",
        us: "United States",
        uy: "Uruguay",
        uz: "Uzbekistan",
        va: "Vatican City",
        vc: "Saint Vincent and the Grenadines",
        ve: "Venezuela",
        vn: "Vietnam",
        vu: "Vanuatu",
        ws: "Samoa",
        xk: "Kosovo",
        ye: "Yemen",
        za: "South Africa",
        zm: "Zambia",
        zw: "Zimbabwe"
      },
      answers: ['', '', '', '', ''],
      correctAnswer: '',
      answersCorrect: 0,
      answersWrong: 0,
      answersTotal: 0,
      imageSource: ""
    }
  },
  methods: {
    resetGame() {
      this.countries = shuffleObject(this.countries)
      this.answersCorrect = 0
      this.answersWrong = 0
      this.answersTotal = 0

      this.loadImage()
    },
    answerClicked(index) {
      if (this.answers[index] === this.correctAnswer) {
        this.answersCorrect++
      } else {
        this.answersWrong++
      }
      this.answersTotal++

      if (this.answersTotal === Object.keys(this.countries).length) {
        this.resetGame()
      } else {
        this.loadImage()
      }
    },
    loadImage() {
      this.imageSource = "https://flagcdn.com/" + Object.keys(this.countries)[this.answersTotal] + ".svg"
      for (let i = 0; i < this.answers.length; i++) {
        let randomIndex = Math.floor(Math.random() * Object.keys(this.countries).length)
        this.answers[i] = this.countries[Object.keys(this.countries)[randomIndex]]
      }
      this.correctAnswer = this.countries[Object.keys(this.countries)[this.answersTotal]]
      this.answers[Math.floor(Math.random() * 5)] = this.correctAnswer
    }
  },
  mounted() {
    this.resetGame()
  }
}
</script>
<style scoped>

#img-container {
  display: flex;
  justify-content: center;
  padding-top: 2rem;
  user-select: none;
}

#stats {
  user-select: none;
  position: absolute;
  top: 2rem;
  right: 2rem;
  text-align: right;
}

#stats p:nth-of-type(1) {
  color: #492386;
}

#stats p:nth-of-type(2) {
  color: green;
}

#stats p:nth-of-type(3) {
  color: #9b0505;
}

#answer-buttons {
  display: flex;
  flex-direction: row;
  justify-content: center;
  gap: 1rem;
  padding-top: 2rem;
}

#answer-buttons button {
  position: static !important;
}

#back-button, #reset-button, button {
  background-color: #090a1a;
  color: #492386;
  margin: 0;
  text-transform: uppercase;
  user-select: none;
  position: absolute;
  width: fit-content;
  height: fit-content;
  border: 1px solid #492386;
  border-radius: 6px;
  padding: .4rem;
  font-size: 1rem;
  top: 1rem;
  left: 1rem;
}

a {
  color: #492386;
  background: none;
  text-decoration: none;
}

a:hover, a:active, a:visited {
  color: #492386;
}

#reset-button {
  bottom: 5%;
  left: 50%;
  transform: translateX(-50%);
  top: auto;
}

@media (max-height: 450px) {
  #reset-button {
    bottom: 0;
  }
}

button:hover {
  background-color: #2c1550 !important;
}
</style>
