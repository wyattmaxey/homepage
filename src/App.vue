<template>
  <div id="app">
    <body>
      <Search :focused="curFocus=='search'" @clicked="focusTerm( 'search' )" @arrow="shiftFocus( 'search', $event )"></Search>
      <div class="wrapper">
        <Term :focused="curFocus=='main'" :links="links.main" ref="main" @clicked="focusTerm( 'main' )" @arrow="shiftFocus( 'main', $event )"></Term>
        <Term :focused="curFocus=='sports'" :links="links.sports" ref="sports" @clicked="focusTerm( 'sports' )" @arrow="shiftFocus( 'sports', $event )"></Term>
        <Term :focused="curFocus=='finance'" :links="links.finance" ref="finance" @clicked="focusTerm( 'finance' )" @arrow="shiftFocus( 'finance', $event )"></Term>
        <Term :focused="curFocus=='reddit'" :links="links.reddit" ref="reddit" @clicked="focusTerm( 'reddit' )" @arrow="shiftFocus( 'reddit', $event )"></Term>
        <Term :focused="curFocus=='dev'" :links="links.dev" ref="dev" @clicked="focusTerm( 'dev' )" @arrow="shiftFocus( 'dev', $event )"></Term>
        <Term :focused="curFocus=='utils'" :links="links.utils" ref="utils" @clicked="focusTerm( 'utils' )" @arrow="shiftFocus( 'utils', $event )"></Term>
      </div>
    </body>
  </div>
</template>

<script>
import Term from './components/Term.vue'
import Search from './components/Search.vue'

export default {
  name: 'app',
  components: {
    Term,
    Search
  },
  data: function() {
    return {
      curFocus: 'search',
      mtx: {
        'search': {
          'left': 'search',
          'right': 'search',
          'up': 'search',
          'down': 'sports'
        },
        'main': {
          'left': 'main',
          'right': 'sports',
          'up': 'search',
          'down': 'reddit'
        },
        'sports': {
          'left': 'main',
          'right': 'finance',
          'up': 'search',
          'down': 'dev'
        },
        'finance': {
          'left': 'sports',
          'right': 'finance',
          'up': 'search',
          'down': 'utils'
        },
        'reddit': {
          'left': 'reddit',
          'right': 'dev',
          'up': 'main',
          'down': 'reddit'
        },
        'dev': {
          'left': 'reddit',
          'right': 'utils',
          'up': 'sports',
          'down': 'dev'
        },
        'utils': {
          'left': 'dev',
          'right': 'utils',
          'up': 'finance',
          'down': 'utils'
        }
      },
      links: {
        'main': [
          {
            'title': 'Email',
            'url': 'https://mail.yahoo.com/d/folders/1'
          },
          {
            'title': 'Weather',
            'url': 'https://www.wbtv.com/weather/'
          },
          {
            'title': 'Maps',
            'url': 'https://www.google.com/maps'
          },
          {
            'title': 'LinkedIn',
            'url': 'https://www.linkedin.com/feed/'
          }
        ],
        'dev': [
          {
            'title': 'GitHub',
            'url': 'https://github.com/'
          },
          {
            'title': 'd3.js',
            'url': 'https://github.com/d3/d3/wiki'
          },
          {
            'title': 'Python',
            'url': 'https://docs.python.org/3/index.html'
          },
          {
            'title': 'FontAwesome',
            'url': 'https://fontawesome.com/cheatsheet'
          }
        ],
        'sports': [
          {
            'title': 'B/R',
            'url': 'https://bleacherreport.com/'
          },
          {
            'title': 'BTP',
            'url': 'https://www.backingthepack.com/'
          },
          {
            'title': 'IPS',
            'url': 'https://insidepacksports.com/forums/8'
          },
          {
            'title': 'PackPride',
            'url': 'https://247sports.com/college/north-carolina-state/'
          },
          {
            'title': 'The Rant',
            'url': 'https://www.tigerdroppings.com/rant/lsu-sports/'
          }
        ],
        'finance': [
          {
            'title': 'BofA',
            'url': 'https://www.bankofamerica.com/'
          },
          {
            'title': 'Amex',
            'url': 'https://www.americanexpress.com/'
          },
          {
            'title': 'Citi',
            'url': 'https://online.citi.com/US/login.do'
          },
          {
            'title': 'Mint',
            'url': 'https://www.mint.com/'
          },
          {
            'title': 'Fidelity',
            'url': 'https://digital.fidelity.com/prgw/digital/login/full-page?AuthRedUrl=https://oltx.fidelity.com/ftgw/fbc/ofsummary/defaultPage'
          },
          {
            'title': 'ML',
            'url': 'https://www.benefits.ml.com/login/login'
          }
        ],
        'utils': [
          {
            'title': 'Alpha Mill',
            'url': 'https://alphamillnc.residentportal.com/resident_portal/?module=authentication&action=view_login'
          },
          {
            'title': 'T-Mobile',
            'url': 'https://prepaid.t-mobile.com/direct-to-account'
          },
          {
            'title': 'Spectrum',
            'url': 'https://www.spectrum.net/'
          },
          {
            'title': 'Duke Energy',
            'url': 'https://www.duke-energy.com/home'
          }
        ],
        'reddit': []
      }
    }
  },
  created() {
    var vm = this;
    var url = 'http://www.reddit.com/r/all/.json?limit=5&json=?';
    var req = new XMLHttpRequest();
    req.onreadystatechange = function() {
      if ( req.readyState == 4 && req.status == 200 ) {
        var data = JSON.parse( req.responseText );
        for( let child of data.data.children ) {
          vm.links.reddit.push( {
            'title': child.data.title.substring( 0, Math.min( child.data.title.length, 20 ) ),
            'url': child.data.url
          } );
        }
      } else if ( req.status != 200 ) {
        console.log( req.responseText );
      }
    }
    req.open( 'GET', url, true );
    req.send();
  },
  methods: {
    focusTerm: function( termStr ) {
      this.curFocus = termStr;
    },
    shiftFocus: function( curTerm, dir ) {
      this.curFocus = this.mtx[curTerm][dir];
    }
  }
}
</script>

<style>
@import url( 'https://fonts.googleapis.com/css?family=Anonymous+Pro&display=swap' );
#app {
  font-family: 'Anonymous Pro', monospace;
  background-color: #2f343f;
}

body {
  background-color: #2f343f;
  color: #676E7D;
  width: 100%;
  min-height: 100%;
  overflow-x: hidden;
}

.wrapper {
  width: 60%;
  margin: 0 auto;
  margin-top: 5%;
  display: grid;
  grid-template-columns: repeat( 3, 1fr );
  grid-gap: 10px;
}
</style>
