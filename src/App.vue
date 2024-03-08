<template>
	<TheHeader> </TheHeader>
	<main class="container">
		<section class="section hero">
			<div class="hero__information">
				<div class="hero__information__texts">
					<h1 class="text-head__primary hero__information__texts__title">
						CPF Generator
					</h1>
					<p>
						Sistema web para geração de CPFs e CNPJs aleatórios para fins de
						teste
					</p>
				</div>
				<button
					class="generate-cpf-button"
					@click="generateCpfAndScrollToContent"
				>
					Gerar CPF
				</button>
				<button
					class="generate-cnpj-button"
					@click="generateCnpjAndScrollToContent"
				>
					Gerar CNPJ
				</button>
			</div>
		</section>
		<section class="section details">
			<div class="generated-cpf-area" v-if="generatedCpf">
				<h2>CPF Gerado:</h2>
				<p>{{ generatedCpf }}</p>
				<div class="copy-button-container">
					<button
						class="copy-cpf-button"
						:class="{ copied: copiedCpf }"
						@click="copyToClipboard(generatedCpf)"
					>
						{{ copiedCpf ? 'Copiado!' : 'Copiar' }}
					</button>
				</div>
			</div>
			<div class="generated-cnpj-area" v-if="generatedCnpj">
				<h2>CNPJ Gerado:</h2>
				<p>{{ generatedCnpj }}</p>
				<div class="copy-button-cnpj-container">
					<button
						class="copy-cnpj-button"
						:class="{ copied: copiedCnpj }"
						@click="copyToClipboard(generatedCnpj)"
					>
						{{ copiedCnpj ? 'Copiado!' : 'Copiar' }}
					</button>
				</div>
			</div>
			<Companies />
			<section class="section hero">
				<div class="lets-talk">
					<div class="lets-talk__texts">
						<h1 class="text-head__contact">Entre em contato</h1>
						<p>
							Sou Rodrigo Braga Desenvolvedor Full Stack, Para mais informações
							entre em contatos pelas redes abaixo, confira também meu portfólio
						</p>
					</div>
					<BaseLink
						class="get-started-link"
						href="https://rodrigobraga-dev.netlify.app/"
						target="_blank"
						>Portifólio</BaseLink
					>
				</div>
				<div class="contact-image">
					<img src="/src/assets/images/Sem título.png" alt="let`s Talk" />
				</div>
			</section>
		</section>
	</main>
	<TheFooter>
		<template #right>
			<UpdateInboxForm :mailer="new MailerServiceImpl()" />
		</template>
	</TheFooter>
</template>

<script setup lang="ts">
	import { ref } from 'vue'
	import UpdateInboxForm from './ui/components/update-inbox-form.vue'
	import TheHeader from './ui/components/the-header.vue'
	import TheFooter from './ui/components/the-footer.vue'
	import Companies from './ui/components/companies.vue'
	import BaseLink from './ui/atoms/base-link.vue'
	import { MailerServiceImpl } from './services/impl/mailer-service'

	let generatedCpf = ref('')
	let generatedCnpj = ref('')
	let copiedCpf = ref(false)
	let copiedCnpj = ref(false)

	const generateCpfAndScrollToContent = () => {
		scrollToContent('.generated-cnpj-area')
		fetch(
			'https://czihgc8hvg.execute-api.us-east-1.amazonaws.com/dev/generate-cpf?type=cpf'
		)
			.then((response) => {
				if (!response.ok) {
					throw new Error('Failed to generate CPF')
				}
				return response.text()
			})
			.then((data) => {
				generatedCpf.value = data.trim()
			})
			.catch((error) => alert('Erro ao gerar CPF: ' + error.message))
	}

	const generateCnpjAndScrollToContent = () => {
		scrollToContent('.generated-cnpj-area')
		fetch(
			'https://czihgc8hvg.execute-api.us-east-1.amazonaws.com/dev/generate-cpf?type=cnpj'
		)
			.then((response) => {
				if (!response.ok) {
					throw new Error('Failed to generate CNPJ')
				}
				return response.text()
			})
			.then((data) => {
				generatedCnpj.value = data.trim()
			})
			.catch((error) => alert('Erro ao gerar CNPJ: ' + error.message))
	}
	const scrollToContent = (selector: string) => {
		const element = document.querySelector(selector)
		if (element) {
			const yOffset = -50
			const y =
				element.getBoundingClientRect().top + window.pageYOffset + yOffset
			window.scrollTo({ top: y, behavior: 'instant' })
		}
	}

	const copyToClipboard = (text: string) => {
		navigator.clipboard
			.writeText(text)
			.then(() => {
				if (text === generatedCpf.value) {
					copiedCpf.value = true
					setTimeout(() => {
						copiedCpf.value = false
					}, 5000)
				} else if (text === generatedCnpj.value) {
					copiedCnpj.value = true
					setTimeout(() => {
						copiedCnpj.value = false
					}, 5000)
				}
			})
			.catch((error) => {
				alert('Erro ao copiar CPF ou CNPJ: ' + error.message)
			})
	}
</script>

<style scoped>
	.generate-cpf-button {
		background-color: #007bff;
		color: white;
		padding: 8px 16px;
		border: none;
		border-radius: 4px;
		cursor: pointer;
	}

	.generate-cpf-button:hover {
		background-color: #0056b3;
	}
	.generated-cpf-area {
		margin-top: 20px;
		padding: 10px;
		border: 1px solid #ccc;
		border-radius: 4px;
	}
	.copy-button-container {
		position: relative;
	}
	.copy-cpf-button {
		position: absolute;
		bottom: 0;
		right: 0;
		margin: 10px;
		background-color: #007bff;
		color: white;
		padding: 8px 16px;
		border: none;
		border-radius: 4px;
		cursor: pointer;
	}

	.copy-cpf-button:hover {
		background-color: #0056b3;
	}
	.copy-cpf-button.copied {
		background-color: #28a745;
	}
	.generate-cnpj-button {
		background-color: #007bff;
		color: white;
		padding: 8px 16px;
		border: none;
		border-radius: 4px;
		cursor: pointer;
	}

	.generate-cnpj-button:hover {
		background-color: #0056b3;
	}
	.generated-cnpj-area {
		margin-top: 20px;
		padding: 10px;
		border: 1px solid #ccc;
		border-radius: 4px;
	}
	.copy-button-cnpj-container {
		position: relative;
	}
	.copy-cnpj-button {
		position: absolute;
		bottom: 0;
		right: 0;
		margin: 10px;
		background-color: #007bff;
		color: white;
		padding: 8px 16px;
		border: none;
		border-radius: 4px;
		cursor: pointer;
	}

	.copy-cnpj-button:hover {
		background-color: #0056b3;
	}
	.copy-cnpj-button.copied {
		background-color: #28a745;
	}

	.header__authentication-links {
		display: flex;
		flex-direction: row;
		align-items: center;
		justify-content: center;
		gap: 30px;
	}
	.container {
		width: 100%;
		max-width: 1200px;
		display: flex;
		flex-direction: column;
		align-items: center;
	}
	.hero {
		flex-direction: row;
		justify-content: space-between;
	}
	.hero__information__texts {
		display: flex;
		flex-direction: column;
		gap: 36px;
	}
	.hero__information__texts__title {
		min-width: 465px;
		text-align: center;
	}
	.text-head__primary {
		font-size: 100px;
	}
	.text-head__contact {
		font-size: 55px;
	}
	.hero__information__texts p {
		text-align: center;
		margin-top: 20px;
	}
	.hero__information {
		flex: 1;
		max-width: 750px;
		display: flex;
		flex-direction: column;
		gap: 20px;
		margin: auto;
	}
	.hero__image {
		width: fit-content;
		height: fit-content;
		border-radius: 20px;
		& > img {
			width: 100%;
		}
	}
	.details {
		flex-direction: column;
	}
	.feature-list {
		display: flex;
		justify-content: space-between;
		gap: 48px;
		padding: 0px;
	}
	.lets-talk {
		flex: 1;
		max-width: 300px;
		display: flex;
		flex-direction: column;
		gap: 44px;
	}
	.lets-talk__texts {
		display: flex;
		flex-direction: column;
		gap: 36px;
	}
	.section {
		display: flex;
		width: 100%;
		gap: 76px;
		padding: 76px 0px;
	}
	.preview-image {
		background-color: var(--dark-gray);
		& > img {
			max-width: 500px;
		}
	}
	.dashboard-image {
		overflow: hidden;
		background-color: #fdf5df;
		& > img {
			position: relative;
			bottom: -7px;
		}
	}
	.contact-image {
		& > img {
			width: 100%;
			max-width: 500px;
		}
	}
	.get-started-link {
		background-color: var(--yellow);
	}

	@media screen and (max-width: 768px) {
		h1,
		h2,
		h3,
		h4,
		h5,
		h6,
		p,
		small {
			text-align: center;
		}
		.header__authentication-links {
			display: none;
		}
		.hero {
			flex-direction: column;
			align-items: center;
		}
		.lets-talk {
			align-items: center;
		}
		.hero__information {
			align-items: center;
		}
		.hero__information__texts__title {
			min-width: auto;
		}
		.preview-image {
			& > img {
				max-width: 100%;
			}
		}
		.contact-image {
			& > img {
				max-width: 100%;
			}
		}
		.feature-list {
			align-items: center;
			flex-direction: column;
		}
	}
	@media screen and (max-width: 560px) {
		.text-head__primary {
			font-size: 40px;
			line-height: 60px;
		}
	}
</style>
