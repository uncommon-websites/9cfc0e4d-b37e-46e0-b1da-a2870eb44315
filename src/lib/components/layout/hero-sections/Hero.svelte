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
		background: white;
		display: flex;
		align-items: center;
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
			radial-gradient(ellipse at top left, rgba(59, 130, 246, 0.03) 0%, transparent 50%),
			radial-gradient(ellipse at bottom right, rgba(147, 51, 234, 0.02) 0%, transparent 50%);
		opacity: 0.6;
	}

	.hero-content {
		position: relative;
		z-index: 10;
	}

	.hero-title {
		font-weight: 600;
		line-height: 1.1;
		letter-spacing: -0.02em;
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
		border-radius: 16px;
		transition: transform 0.3s ease;
		max-width: 600px;
	}

	.hero-image:hover {
		transform: scale(1.02);
	}

	:global(.hero-button) {
		transition: all 0.2s ease;
	}

	:global(.hero-button:hover) {
		transform: translateY(-1px);
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
