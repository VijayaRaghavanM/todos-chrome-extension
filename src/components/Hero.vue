<template>
	<div>
    	<section class="hero is-primary">
  			<div class="hero-body">
    			<div class="container">
			      <h1 class="title">
			        <slot></slot>
			      </h1>
    			</div>
  			</div>
		</section>
  	</div>
</template>

<script>
export default {
	name: "hero",
};
</script>