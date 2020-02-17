<template>
  <div ref="term_div" class="term-div" :tabIndex="-1"
  	@keyup.up="decrement()" @keyup.down="increment()" @keyup.enter="openPage()"
  	@keyup.shift.up="$emit( 'arrow', 'up' )" @keyup.shift.down="$emit( 'arrow', 'down' )"
  	@keyup.shift.left="$emit( 'arrow', 'left' )" @keyup.shift.right="$emit( 'arrow', 'right' )"
  	@click="$emit( 'clicked' )">
  	<ul>
  		<li v-for="link in linkIndexer" :class="{ 'active-item': focused && curIdx==link.idx }">
  			<a :href="link.url" class="term-link" :class="{ 'term-link-active': focused && curIdx==link.idx }">{{ link.title }}</a>
  		</li>
  	</ul>
  </div>
</template>

<script>
export default {
  name: 'Term',
  props: ['focused', 'links'],
  data: function() {
  	return {
  		curIdx: 0
  	}
  },
  computed: {
  	linkIndexer: function() {
  		var linksIdx = [];
  		for ( var i in this.links ) {
  			var link = this.links[i];
  			link.idx = i;
  			linksIdx.push( link );
  		}
  		return linksIdx;
  	}
  },
  watch: {
  	focused: function( val ) {
  		var vm = this;
  		if ( val===true ) {
  			vm.$nextTick( () => {
		      vm.$refs.term_div.focus();
		    } );
  		} else {
  			vm.$nextTick( () => {
		      vm.$refs.term_div.blur();
		    } );
  		}
  	}
  },
  methods: {
  	increment: function() {
  		this.curIdx++;
  		if ( this.curIdx >= this.linkIndexer.length ) {
  			this.curIdx = 0;
  		}
  	},
  	decrement: function() {
  		this.curIdx--;
  		if ( this.curIdx < 0 ) {
  			this.curIdx = this.linkIndexer.length - 1;
  		}
  	},
  	openPage: function() {
  		var vm = this;
  		window.open( vm.linkIndexer[vm.curIdx].url, '_blank' );
  	}
  }
}
</script>

<style scoped>
.term-div {
	border-style: solid;
	border-width: 3px;
	border-color: #4B515E;
	background-color: #2f343f;
	padding: 5px;
	width: 80%;
	height: 80%;
}

.term-div:focus {
	outline: none;
	display: block;
	border-style: solid;
	border-width: 3px;
	border-color: #676E7D;
	background-color: #2f343f;
}

ul {
	list-style-type: none;
	padding: 0px;
	margin-top: 2px;
}

li {
	margin-bottom: 2px;
	padding: 1px;
}

a {
	text-decoration: none;
}

a:hover {
	cursor: default;
}

.term-link {
	color: #676E7D;
}

.term-link-active {
	color: #fff;
}

.active-item {
	background-color: #676E7D;
}
</style>
