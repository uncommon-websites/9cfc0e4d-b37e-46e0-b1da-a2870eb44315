<!--
    @component
    Clean, modern hero banner with compelling text and strong call-to-action. NEVER use title case.

    Usage:
    ```html
    <Hero
      title="Bold Claim"
      subtitle="Quick Value"
      imageSrc="/hero.jpg"
      callsToAction={[
        {
          href: "/start",
          label: "Go"
        },
        {
          href: "/learn",
          label: "More"
        }
      ]}
    />
    ```

    Props:
    - `title`: Main headline (string)
    - `subtitle`: Supporting text (string)
    - `imageSrc`: Hero image URL (string)
    - `callsToAction`: CTA buttons array (max two: primary, secondary)
-->

<script lang="ts">
	// Components
	import AnimateText from "$lib/components/animation/AnimateText.svelte";
	import Button from "$lib/components/ui/Button.svelte";

	// Constants
	import { cta } from "$lib/navigation";

	// Types
	type Props = {
		centered?: boolean;
		title: string;
		subtitle: string;
		imageSrc?: string;
		callsToAction?: Array<{
			href: string;
			label: string;
		}>; // A maximum of two calls to action, with the first one being primary and the second one being secondary
	};

	let {
		title,
		subtitle,
		imageSrc,
		callsToAction = [cta],
		centered = false,
		...rest
	}: Props = $props();
</script>

<div class="hero-container" {...rest}>
	<!-- Clean background with subtle gradient -->
	<div class="hero-bg">
		<div class="gradient-overlay"></div>
		<div class="geometric-elements">
			<div class="floating-circle circle-1"></div>
			<div class="floating-circle circle-2"></div>
			<div class="floating-circle circle-3"></div>
			<div class="grid-pattern"></div>
		</div>
	</div>

	<!-- Content -->
	<header
		class={[
			"hero-content section-px relative z-10 container mx-auto grid items-center gap-16 gap-y-12 py-20 text-balance",
			centered ? "place-items-center text-center" : "xl:grid-cols-[1fr_1fr]"
		]}
		data-enter-container
	>
		<div class="grid gap-8" class:max-w-prose={centered}>
			<h1 class="text-display hero-title w-full" data-enter>
				<span class="block"><AnimateText text={title} /></span>
			</h1>

			<p
				data-enter
				class={[
					"text-muted-foreground text-headline block max-w-[50ch] transition duration-500 ease-out",
					centered ? "mx-auto" : ""
				]}
			>
				{subtitle}
			</p>

			{#if callsToAction.length > 0}
				<div class="flex gap-4" data-enter class:justify-center={centered}>
					{#each callsToAction as cta, index}
						<Button
							href={cta.href}
							size="lg"
							variant={index % 2 === 0 ? "primary" : "secondary"}
							class="hero-button max-lg:hidden">{cta.label}</Button
						>
						<Button
							href={cta.href}
							size="md"
							variant={index % 2 === 0 ? "primary" : "secondary"}
							class="hero-button lg:hidden">{cta.label}</Button
						>
					{/each}
				</div>
			{/if}
		</div>

		{#if imageSrc && !centered}
			<div class="hero-image-container" data-enter>
				<img
					src={imageSrc}
					alt="Team collaboration"
					class="hero-image"
					loading="eager"
				/>
			</div>
		{/if}
	</header>
</div>

<style>
	.hero-container {
		min-height: 100vh;
		position: relative;
		overflow: hidden;
		background: 
			linear-gradient(135deg, #ffffff 0%, #fafafa 50%, #ffffff 100%);
		display: flex;
		align-items: center;
		box-shadow: inset 0 0 100px rgba(59, 130, 246, 0.02);
	}

	.hero-bg {
		position: absolute;
		top: 0;
		left: 0;
		width: 100%;
		height: 100%;
	}

	.gradient-overlay {
		position: absolute;
		top: 0;
		left: 0;
		width: 100%;
		height: 100%;
		background: 
			radial-gradient(ellipse 120% 80% at 20% 10%, rgba(59, 130, 246, 0.08) 0%, transparent 60%),
			radial-gradient(ellipse 100% 120% at 80% 90%, rgba(147, 51, 234, 0.06) 0%, transparent 60%),
			radial-gradient(ellipse 80% 100% at 50% 50%, rgba(16, 185, 129, 0.03) 0%, transparent 50%),
			linear-gradient(135deg, rgba(59, 130, 246, 0.02) 0%, rgba(147, 51, 234, 0.02) 100%);
		opacity: 0.9;
		animation: gradientShift 20s ease-in-out infinite;
	}

	@keyframes gradientShift {
		0%, 100% {
			transform: scale(1) rotate(0deg);
			opacity: 0.9;
		}
		33% {
			transform: scale(1.05) rotate(1deg);
			opacity: 0.7;
		}
		66% {
			transform: scale(0.95) rotate(-1deg);
			opacity: 1;
		}
	}

	.geometric-elements {
		position: absolute;
		top: 0;
		left: 0;
		width: 100%;
		height: 100%;
		overflow: hidden;
		pointer-events: none;
	}

	.floating-circle {
		position: absolute;
		border-radius: 50%;
		background: linear-gradient(135deg, rgba(59, 130, 246, 0.1), rgba(147, 51, 234, 0.05));
		backdrop-filter: blur(1px);
	}

	.circle-1 {
		width: 300px;
		height: 300px;
		top: -150px;
		right: -150px;
		animation: float1 25s ease-in-out infinite;
	}

	.circle-2 {
		width: 200px;
		height: 200px;
		bottom: -100px;
		left: -100px;
		animation: float2 30s ease-in-out infinite reverse;
	}

	.circle-3 {
		width: 150px;
		height: 150px;
		top: 40%;
		right: 10%;
		animation: float3 20s ease-in-out infinite;
	}

	.grid-pattern {
		position: absolute;
		top: 0;
		left: 0;
		width: 100%;
		height: 100%;
		background-image: 
			linear-gradient(rgba(59, 130, 246, 0.02) 1px, transparent 1px),
			linear-gradient(90deg, rgba(59, 130, 246, 0.02) 1px, transparent 1px);
		background-size: 60px 60px;
		animation: gridMove 40s linear infinite;
		opacity: 0.3;
	}

	@keyframes float1 {
		0%, 100% {
			transform: translate(0, 0) rotate(0deg) scale(1);
		}
		33% {
			transform: translate(-30px, -20px) rotate(120deg) scale(1.1);
		}
		66% {
			transform: translate(20px, -30px) rotate(240deg) scale(0.9);
		}
	}

	@keyframes float2 {
		0%, 100% {
			transform: translate(0, 0) rotate(0deg) scale(1);
		}
		50% {
			transform: translate(40px, -40px) rotate(180deg) scale(1.2);
		}
	}

	@keyframes float3 {
		0%, 100% {
			transform: translate(0, 0) rotate(0deg) scale(1);
		}
		25% {
			transform: translate(-20px, 30px) rotate(90deg) scale(0.8);
		}
		75% {
			transform: translate(30px, -20px) rotate(270deg) scale(1.1);
		}
	}

	@keyframes gridMove {
		0% {
			transform: translate(0, 0);
		}
		100% {
			transform: translate(60px, 60px);
		}
	}

	.hero-content {
		position: relative;
		z-index: 10;
	}

	.hero-title {
		font-weight: 600;
		line-height: 1.1;
		letter-spacing: -0.02em;
		background: linear-gradient(135deg, #1e293b 0%, #475569 50%, #1e293b 100%);
		background-clip: text;
		-webkit-background-clip: text;
		-webkit-text-fill-color: transparent;
		background-size: 200% 200%;
		animation: titleShimmer 8s ease-in-out infinite;
	}

	@keyframes titleShimmer {
		0%, 100% {
			background-position: 0% 50%;
		}
		50% {
			background-position: 100% 50%;
		}
	}

	.hero-image-container {
		position: relative;
		display: flex;
		align-items: center;
		justify-content: center;
	}

	.hero-image {
		width: 100%;
		height: auto;
		border-radius: 20px;
		transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
		max-width: 600px;
		box-shadow: 
			0 25px 50px -12px rgba(0, 0, 0, 0.08),
			0 0 0 1px rgba(255, 255, 255, 0.05);
		position: relative;
	}

	.hero-image:hover {
		transform: scale(1.03) translateY(-8px);
		box-shadow: 
			0 35px 70px -12px rgba(0, 0, 0, 0.12),
			0 0 0 1px rgba(255, 255, 255, 0.1);
	}

	.hero-image-container::before {
		content: '';
		position: absolute;
		top: -20px;
		left: -20px;
		right: -20px;
		bottom: -20px;
		background: linear-gradient(135deg, rgba(59, 130, 246, 0.1), rgba(147, 51, 234, 0.05));
		border-radius: 30px;
		z-index: -1;
		opacity: 0;
		transition: opacity 0.4s ease;
	}

	.hero-image-container:hover::before {
		opacity: 1;
	}

	:global(.hero-button) {
		transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
		position: relative;
		overflow: hidden;
	}

	:global(.hero-button:hover) {
		transform: translateY(-2px) scale(1.02);
		box-shadow: 0 10px 25px -5px rgba(0, 0, 0, 0.1);
	}

	:global(.hero-button::before) {
		content: '';
		position: absolute;
		top: 0;
		left: -100%;
		width: 100%;
		height: 100%;
		background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.2), transparent);
		transition: left 0.5s;
	}

	:global(.hero-button:hover::before) {
		left: 100%;
	}

	/* Responsive adjustments */
	@media (max-width: 1200px) {
		.hero-content {
			grid-template-columns: 1fr;
			text-align: center;
			gap: 12;
		}
		
		.hero-image-container {
			order: -1;
			margin-bottom: 2rem;
		}
	}

	@media (max-width: 768px) {
		.hero-container {
			min-height: 90vh;
		}
		
		.hero-content {
			padding-top: 3rem;
			padding-bottom: 3rem;
			gap: 8;
		}
		
		.hero-image {
			border-radius: 12px;
		}
	}
</style>
