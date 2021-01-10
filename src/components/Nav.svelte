<script lang="ts">
	export let segment: string;

	import { goto } from '@sapper/app';

	const navLinks = function(): Array<{href: string, selected: boolean}> {
		const aLinks = Array.from(document.querySelectorAll("#menu li a"));
		return aLinks.map(a => {
			return {href: a.getAttribute("href"), selected: a.getAttribute("aria-current") == "page"}
		});
	}

	const linksWithSelectedI = function(): [number, Array<{href: string, selected: boolean}>]{
		const links = navLinks();
		const selected = links.find(l => l.selected);
		const selectedI = links.indexOf(selected);
		return [selectedI, links];
	}

	const next = function(){
		const [selectedI, links] = linksWithSelectedI();
		const nextI = selectedI == (links.length - 1) ? 0: selectedI + 1;
		return links[nextI].href;
	}

	const prev = function(){
		const [selectedI, links] = linksWithSelectedI();
		const prevI = selectedI == 0 ? links.length - 1: selectedI - 1;
		return links[prevI].href;
	}

	const navigate = function(event){
		// the user is not writing in form input
		console.log(event.key)
		if (event.target.type === undefined){
			if (['n', 'arrowright'].includes(event.key.toLowerCase())) {
				goto(next());
			} else if (['p', 'arrowleft'].includes(event.key.toLowerCase())){
				goto(prev());
			}
		}
	}

</script>

<style>
	nav {
		border-bottom: 1px solid rgba(255,62,0,0.1);
		font-weight: 300;
		padding: 0 1em;
	}

	ul {
		margin: 0;
		padding: 0;
		display: flex;
  	align-items: center;
  	justify-content: center;
		flex-direction: row;
	}

	/* clearfix */
	ul::after {
		content: '';
		display: block;
		clear: both;
	}

	li {
		display: block;
		/*float: left;*/
	}

	[aria-current] {
		position: relative;
		display: inline-block;
	}

	[aria-current]::after {
		position: absolute;
		content: '';
		width: calc(100% - 1em);
		height: 2px;
		background-color: rgb(255,62,0);
		display: block;
		bottom: -1px;
	}

	a {
		text-decoration: none;
		padding: 1em 0.5em;
		display: block;
	}
</style>

<nav>
	<ul id="menu">
		<li><a aria-current="{segment === undefined ? 'page' : undefined}" href=".">Today</a></li>
		<li><a aria-current="{segment === 'interval' ? 'page' : undefined}" href="interval">Date Interval</a></li>
		<li><a aria-current="{segment === 'vat' ? 'page' : undefined}" href="vat">VAT</a></li>

		<!-- for the blog link, we're using rel=prefetch so that Sapper prefetches
		     the blog data when we hover over the link or tap it on a touchscreen -->
		<!--<li><a rel=prefetch aria-current="{segment === 'blog' ? 'page' : undefined}" href="blog">blog</a></li>-->
	</ul>
</nav>

<svelte:window on:keyup={navigate}/>