<template>
	<footer class="footer">
		<TheLogo />
		<section class="footer__content">
			<ul class="footer__content__links">
				<li>
					<TextLink aria-disabled="true" class="copyright"
						>© Rodrigo Braga 2024</TextLink
					>
				</li>
				<li>
					<TextLink
						href="https://www.linkedin.com/in/rodrigobraga0125/"
						target="_blank"
						>Linkedin</TextLink
					>
				</li>
				<li>
					<TextLink
						href="https://www.instagram.com/rodrigobraga_dev/"
						target="_blank"
						>Instagram</TextLink
					>
				</li>
				<li>
					<TextLink href="https://github.com/rodrigoCode-tech" target="_blanck"
						>Github</TextLink
					>
				</li>
			</ul>
			<slot name="right"></slot>
		</section>
	</footer>
</template>

<script setup lang="ts">
	import TextLink from '../atoms/text-link.vue'
	import TheLogo from './the-logo.vue'
</script>

<style scoped>
	.footer {
		width: 100%;
		display: flex;
		flex-direction: column;
		padding: 56px 0px;
		border-top: 1px solid #ffffff33;
	}
	.footer__content {
		flex: 1;
		display: flex;
		flex-direction: row;
		justify-content: space-between;
		align-items: flex-start;
		flex-wrap: wrap-reverse;
		gap: 56px;
	}
	.footer__content__links {
		display: flex;
		flex-direction: row;
		gap: 24px;
		min-width: 360px;
	}
	.copyright {
		cursor: default;
		&:hover {
			opacity: 1;
		}
	}

	@media screen and (max-width: 983px) {
		.footer {
			gap: 56px;
		}
	}

	@media screen and (max-width: 560px) {
		.footer__content__links {
			flex-direction: column;
		}
	}
</style>
