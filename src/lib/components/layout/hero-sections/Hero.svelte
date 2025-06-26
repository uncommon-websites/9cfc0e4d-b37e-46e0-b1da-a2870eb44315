<!--
    @component
    Bold hero banner with eye-catching text and strong call-to-action. NEVER use title case.

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

	// Remove image error handler since we're not using images

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

<div class="synthwave-hero" {...rest}>
	<!-- Synthwave animated background -->
	<div class="synthwave-bg">
		<!-- Grid lines -->
		<div class="grid-lines">
			<div class="grid-horizontal"></div>
			<div class="grid-vertical"></div>
		</div>
		
		<!-- Geometric shapes -->
		<div class="geometric-shapes">
			<div class="triangle triangle-1"></div>
			<div class="triangle triangle-2"></div>
			<div class="circle circle-1"></div>
			<div class="circle circle-2"></div>
			<div class="line line-1"></div>
			<div class="line line-2"></div>
		</div>
		
		<!-- Scanlines -->
		<div class="scanlines"></div>
	</div>

	<!-- Content -->
<header
		class={[
			"synthwave-content section-px container mx-auto grid items-center gap-16 gap-y-9 py-12 text-balance relative z-10",
			centered ? "place-items-center text-center" : " xl:grid-cols-[1fr_auto]"
		]}
		data-enter-container
	>
		<div class="grid gap-6" class:max-w-prose={centered}>
			<h1 class="text-display w-full synthwave-title" data-enter>
				<span class="block"><AnimateText text={title} /></span>
				{#if !centered}
					<span class="text-emphasis-dim block"><AnimateText text={subtitle} /></span>
				{/if}
			</h1>

			{#if centered}
				<p
					data-enter
					class={[
						"text-muted-foreground text-headline mx-auto block max-w-[45ch] transition duration-500 ease-out"
						// isTitleComplete ? "opacity-100" : "translate-y-2 opacity-0 blur-sm"
					]}
				>
					{subtitle}
				</p>
			{/if}
		</div>

		{#if callsToAction.length > 0}
			<div class="flex gap-4" data-enter>
				{#each callsToAction as cta, index}
					<Button
						href={cta.href}
						size="lg"
						variant={index % 2 === 0 ? "primary" : "secondary"}
						class="max-lg:hidden synthwave-button">{cta.label}</Button>
					<Button
						href={cta.href}
						size="md"
						variant={index % 2 === 0 ? "primary" : "secondary"}
						class="lg:hidden synthwave-button">{cta.label}</Button>
				{/each}
			</div>
		{/if}
	</header>

</div>

<style>
	.synthwave-hero {
		@apply bg-white relative overflow-hidden;
		min-height: 100vh;
		display: flex;
		align-items: center;
	}

	.synthwave-bg {
		position: absolute;
		top: 0;
		left: 0;
		width: 100%;
		height: 100%;
		opacity: 0.1;
	}

	/* Grid Lines */
	.grid-lines {
		position: absolute;
		top: 0;
		left: 0;
		width: 100%;
		height: 100%;
	}

	.grid-horizontal {
		position: absolute;
		top: 0;
		left: 0;
		width: 100%;
		height: 100%;
		background-image: repeating-linear-gradient(
			0deg,
			transparent,
			transparent 49px,
			currentColor 50px
		);
		animation: grid-move-horizontal 20s linear infinite;
	}

	.grid-vertical {
		position: absolute;
		top: 0;
		left: 0;
		width: 100%;
		height: 100%;
		background-image: repeating-linear-gradient(
			90deg,
			transparent,
			transparent 49px,
			currentColor 50px
		);
		animation: grid-move-vertical 15s linear infinite;
	}

	/* Geometric Shapes */
	.geometric-shapes {
		position: absolute;
		top: 0;
		left: 0;
		width: 100%;
		height: 100%;
	}

	.triangle {
		position: absolute;
		width: 0;
		height: 0;
		border-style: solid;
		animation: triangle-float 8s ease-in-out infinite;
	}

	.triangle-1 {
		top: 20%;
		left: 10%;
		border-left: 30px solid transparent;
		border-right: 30px solid transparent;
		border-bottom: 52px solid currentColor;
		animation-delay: 0s;
	}

	.triangle-2 {
		top: 60%;
		right: 15%;
		border-left: 20px solid transparent;
		border-right: 20px solid transparent;
		border-bottom: 35px solid currentColor;
		animation-delay: 4s;
		transform: rotate(180deg);
	}

	.circle {
		position: absolute;
		border: 2px solid currentColor;
		border-radius: 50%;
		animation: circle-pulse 6s ease-in-out infinite;
	}

	.circle-1 {
		top: 30%;
		right: 20%;
		width: 60px;
		height: 60px;
		animation-delay: 1s;
	}

	.circle-2 {
		bottom: 25%;
		left: 20%;
		width: 40px;
		height: 40px;
		animation-delay: 3s;
	}

	.line {
		position: absolute;
		background: currentColor;
		animation: line-grow 10s ease-in-out infinite;
	}

	.line-1 {
		top: 40%;
		left: 5%;
		width: 2px;
		height: 100px;
		animation-delay: 2s;
	}

	.line-2 {
		bottom: 30%;
		right: 10%;
		width: 80px;
		height: 2px;
		animation-delay: 5s;
	}

	/* Scanlines */
	.scanlines {
		position: absolute;
		top: 0;
		left: 0;
		width: 100%;
		height: 100%;
		background: repeating-linear-gradient(
			0deg,
			transparent,
			transparent 2px,
			currentColor 2px,
			currentColor 4px
		);
		opacity: 0.03;
		animation: scanlines-move 3s linear infinite;
	}

	/* Content Styling */
	.synthwave-content {
		position: relative;
		z-index: 10;
	}

	.synthwave-title {
		text-shadow: 0 0 10px currentColor;
		animation: title-glow 4s ease-in-out infinite alternate;
	}

	:global(.synthwave-button) {
		box-shadow: 0 0 20px rgba(0, 0, 0, 0.3);
		transition: all 0.3s ease;
	}

	:global(.synthwave-button:hover) {
		box-shadow: 0 0 30px rgba(0, 0, 0, 0.5);
		transform: translateY(-2px);
	}

	/* Animations */
	@keyframes grid-move-horizontal {
		0% { transform: translateY(0); }
		100% { transform: translateY(50px); }
	}

	@keyframes grid-move-vertical {
		0% { transform: translateX(0); }
		100% { transform: translateX(50px); }
	}

	@keyframes triangle-float {
		0%, 100% { transform: translateY(0) rotate(0deg); }
		25% { transform: translateY(-20px) rotate(5deg); }
		50% { transform: translateY(-10px) rotate(-3deg); }
		75% { transform: translateY(-15px) rotate(2deg); }
	}

	@keyframes circle-pulse {
		0%, 100% { 
			transform: scale(1);
			opacity: 0.5;
		}
		50% { 
			transform: scale(1.2);
			opacity: 0.8;
		}
	}

	@keyframes line-grow {
		0%, 100% { 
			transform: scale(1);
			opacity: 0.3;
		}
		50% { 
			transform: scale(1.5);
			opacity: 0.7;
		}
	}

	@keyframes scanlines-move {
		0% { transform: translateY(0); }
		100% { transform: translateY(4px); }
	}

	@keyframes title-glow {
		0% { text-shadow: 0 0 10px currentColor; }
		100% { text-shadow: 0 0 20px currentColor, 0 0 30px currentColor; }
	}

	/* Responsive adjustments */
	@media (max-width: 768px) {
		.triangle-1 {
			border-left-width: 20px;
			border-right-width: 20px;
			border-bottom-width: 35px;
		}
		
		.triangle-2 {
			border-left-width: 15px;
			border-right-width: 15px;
			border-bottom-width: 26px;
		}
		
		.circle-1 {
			width: 40px;
			height: 40px;
		}
		
		.circle-2 {
			width: 30px;
			height: 30px;
		}
		
		.line-1 {
			height: 60px;
		}
		
		.line-2 {
			width: 50px;
		}
	}
</style>
