<template>
  <div id="app" @keypress="bgChange" :class="qcopy == query ? chosenimg = imgs[Math.floor(Math.random() * 7) + 1] : chosenimg">
    <main>
      <div class="search-box">
        <input 
        type="text" 
        class="search-bar" 
        placeholder="Ask Ron..."
        v-model="query"
        @keypress="fetchQuote"
        />
      </div>

      <div class="ron-wrap" v-if="quote.length > 0">
        <div class="today-box">
          <div class="ron-says">{{ qdisplay }} Ron Swanson has some thoughts...</div>
        </div>

        <div class="ron-quote">
          <div class="quote">{{ quote }}</div>
        </div>
        <footer class="ur-welcome">You're welcome.</footer>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: 'App',
  data () {
    return {
      api_base: 'https://ron-swanson-quotes.herokuapp.com/v2/quotes',
      query: '',
      qcopy: '',
      qdisplay: '',
      quote: {},
      count: 0,
      imgs: {
        1: 'one',
        2: 'two',
        3: 'three',
        4: 'four',
        5: 'five',
        6: 'six',
        7: 'seven',
        8: 'eight'
      },
      chosenimg: ''
    }
  },
  methods: {
    fetchQuote (e) {
      if (e.key === "Enter") {
        fetch(`${this.api_base}/search/${this.query}`)
          .then(res => {
            return res.json()
          }).then(this.setQuote);
      }
    },
    setQuote (results) {
      if ( results.length == 0 ){
        this.quote = `SIKE! Don't be ridiculous, Ron would never have a thought about "${this.query}"`
        console.log(`No results matching ${this.query}`);
      } else {
        var chosen = results[Math.floor(Math.random() * results.length)]; 
        this.quote = chosen;
      }
    },
    bgChange(e){
      if (e.key === "Enter"){
        if (this.query == ''){
          this.fetchQuote(e);
        }
        this.qcopy = this.query;
        if (this.query == ''){
          this.qdisplay = this.query;
        } else {
          this.qdisplay = this.query + '?';
        }
      }
    }
  }
}
</script>

<style>
  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    overflow: hidden;
  }

  body{
    font-family: 'monoserrat', monospace;
    text-align: center;
  }

  #app {
    background-image: url('./assets/ron_grin.jpeg');
    background-size: cover;
    background-position: bottom;
    transition: 0.4s;
  }

  #app.one {
    background-image: url('./assets/ron_cornrow.jpeg');
  }

  #app.two {
    background-image: url('./assets/ron_hat.png');
  }

  #app.three {
    background-image: url('./assets/ron_stern.jpeg');
  }

  #app.four {
    background-image: url('./assets/ron_trophy.jpeg');
  }

  #app.five {
    background-image: url('./assets/ron_frazzled.jpeg');
  }

  #app.six {
    background-image: url('./assets/ron_drink.jpeg');
  }

  #app.seven {
    background-image: url('./assets/ron_disgusted.jpeg');
  }

  #app.eight {
    background-image: url('./assets/ron_covered.jpeg');
  }

  main {
    min-height: 100vh;
    padding: 25px;

    background-image: linear-gradient(to bottom, rgba(0,0,0,0.50), rgba(0,0,0,0.75))
  }

  .search-box {
    width: 100%;
    margin-bottom: 30px;
  }

  .search-box .search-bar{
    display: block;
    width: 100%;
    padding: 15px;

    color: #313131;
    font-size: 20px;

    appearance: none;
    border: none;
    outline: none;
    background: none;

    box-shadow: 0px 0px 16px rgba(0,0,0,0.25);
    background-color: rgba(255, 255, 255, 0.5);
    border-radius: 16px 16px 16px 16px;
  }

  .search-box .search-bar:focus{
    box-shadow: 0px 0px 16px rgba(0,0,0,0.25);
    background-color: rgba(255, 255, 255, 0.75);
  }

  .today-box .ron-says{
    color: #ffffff;
    font-size: 30px;
    font-weight: 300;
    text-align: center;
    text-shadow: 1px 3px rgba(0,0,0,0.25);
  }

  .ron-quote {
    text-align: center;
  }

  .ron-quote .quote{
    display: inline-block;
    padding: 10px 25px;
    color:#ffffff;
    font-size: 30px;
    font-weight: 900;

    text-shadow: 1px 3px rgba(0,0,0,0.25);
    background-color: rgba(255, 255, 255, 0.1);
    border-radius: 16px;
    margin: 30px 0px;
    margin-top: 10vh;
  }

  footer{
    display: inline-block;
    padding: 10px 25px;
    color:#ffffff;
    font-size: 20px;
    font-weight: 700;

    text-shadow: 1px 3px rgba(0,0,0,0.25);
    border-radius: 16px;
    margin: 30px 0px;
  }  

  
</style>
