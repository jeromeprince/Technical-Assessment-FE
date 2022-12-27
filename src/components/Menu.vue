<script setup lang="ts">
import axios from "axios";
import { ref } from "vue";
import { onMounted } from "vue";

const glacesses = ref({
	total: 0,
	collections: [],
});
const base_url =
	"https://staging-api.bloobloom.com/user/v1/sales_channels/website/collections";

onMounted(() => {
	loadGlacesses();
});

const loadGlacesses = () => {
	axios
		.get(base_url)
		.then((r) => {
			glacesses.value.total = r.data.meta.total_count;
			glacesses.value.collections = r.data.collections;
		})
		.catch((e) => {
			console.log(e.response);
		});
};
</script>
<template>
	<div class="top-menu">
		<!-- <span class="toogle-side-bar">Menu</span>
    <span><img src="@/assets/logo.svg" alt="logo" class="logo-icon" /></span> -->
		<div class="toogle-side-bar">
			<ul class="main-nav position">
				<li class="has-submenu">
					<a class="" href="#">Home</a>
					<ul class="submenu">
						<li class="has-submenu">
							<a href="#">WONDER</a>
							<ul class="submenu">
								<li><a href="#">MAN</a></li>
								<li><a href="#">WOMAN</a></li>
							</ul>
						</li>
						<li class="has-submenu">
							<a href="#">VISIONARY</a>
							<ul class="submenu">
								<li><a href="#">MAN</a></li>
								<li><a href="#">WOMAN</a></li>
							</ul>
						</li>

						<li class="has-submenu">
							<a href="#">TRAVELLER</a>
							<ul class="submenu">
								<li><a href="#">MAN</a></li>
								<li><a href="#">WOMAN</a></li>
							</ul>
						</li>
					</ul>
				</li>
			</ul>
			<img src="@/assets/logo.svg" alt="logo" class="logo-icon" />
		</div>
		<span><img src="@/assets/logo.svg" alt="logo" class="logo-icon" /></span>
	</div>
</template>
