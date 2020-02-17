<template>
  <div ref="search_div" class="run-div" :class="{ 'focused-div': focused }" @click="$emit( 'clicked' )" :tabIndex="-1"
  	@keyup.shift.up="$emit( 'arrow', 'up' )" @keyup.shift.down="$emit( 'arrow', 'down' )"
  	@keyup.shift.left="$emit( 'arrow', 'left' )" @keyup.shift.right="$emit( 'arrow', 'right' )">
  	<label for="search-in">search: </label>
  	<input ref="search_in" v-model="qry" @keyup.enter="openTab()" type="text" class="search-in" name="search-in"/>
  </div>
</template>

<script>
export default {
  name: 'Search',
  props: ['focused'],
  data: function() {
  	return {
  		qry: ''
  	}
  },
  watch: {
  	focused: function( val ) {
  		var vm = this;
  		if ( val===true ) {
  			vm.$nextTick( () => {
		      vm.$refs.search_in.focus();
		    } );
  		} else {
  			vm.$nextTick( () => {
		      vm.$refs.search_in.blur();
		    } );
  		}
  	}
  },
  methods: {
  	openTab: function() {
  		var vm = this;
  		var baseURL = 'http://google.com/search?q=';
  		window.open( baseURL + vm.qry, '_blank' );
  		vm.qry = '';
  	}
  },
  mounted() {
  	var vm = this;
  	vm.$nextTick( () => {
      vm.$refs.search_in.focus();
    } );
  }
}
</script>

<style scoped>
.run-div {
	display: block;
	border-style: solid;
	border-width: 3px;
	border-color: #4B515E;
	background-color: #2f343f;
	width: 20%;
	margin: 0 auto;
	padding: 4px;
}

.run-div:focus {
	outline: none;
}

.focused-div {
	display: block;
	border-style: solid;
	border-width: 3px;
	border-color: #676E7D;
	background-color: #2f343f;
}

.search-in {
	font-family: 'Anonymous Pro', monospace;
	color: #676E7D;
	background-color: #2f343f;
	border-style: none;
	font-size: 1em;
}

.search-in:focus {
	outline: none;
}
</style>
