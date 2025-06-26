<script lang="ts">
	import { onMount } from 'svelte';

	// Types
	import type { ComponentType } from "svelte";
	
	type Feature = {
		title: string;
		description: string;
		icon?: ComponentType;
		iconClass?: string;
		imageSrc?: string;
		imageAspect?: "16/9" | "9/16";
		link?: {
			href: string;
			label: string;
		};
	};

	// Components
	import SectionHeader from "./SectionHeader.svelte";

	// Props
	const {
		title,
		subtitle,
		features = [],
		...rest
	}: { title: string; subtitle: string; features: Feature[] } = $props();

	let activeFeatureIndex = $state(0);
	let sectionElement: HTMLElement;

	// Abstract metaphor animations for each feature
	const metaphors = [
		{
			// Real-time collaboration - flowing streams converging
			title: "Flowing streams",
			description: "Multiple currents merge into one powerful river",
			animation: "streams"
		},
		{
			// Design-dev bridge - connecting islands
			title: "Bridging worlds",
			description: "Two distant islands connected by a graceful arc",
			animation: "bridge"
		},
		{
			// AI assistance - growing constellation
			title: "Guiding constellation",
			description: "Stars align to illuminate the path forward",
			animation: "constellation"
		},
		{
			// Work anywhere - floating clouds
			title: "Boundless clouds",
			description: "Ideas drift freely across infinite skies",
			animation: "clouds"
		},
		{
			// Design systems - crystalline structure
			title: "Crystal lattice",
			description: "Perfect patterns repeat and strengthen the whole",
			animation: "crystal"
		},
		{
			// Interactive prototypes - morphing shapes
			title: "Living forms",
			description: "Shapes breathe and transform with intention",
			animation: "morph"
		}
	];

	onMount(() => {
		const handleScroll = () => {
			if (!sectionElement) return;

			const rect = sectionElement.getBoundingClientRect();
			const sectionHeight = rect.height;
			const viewportHeight = window.innerHeight;
			
			// Calculate how far we've scrolled through the section
			const scrollProgress = Math.max(0, Math.min(1, 
				(viewportHeight - rect.top) / (sectionHeight + viewportHeight)
			));

			// Determine which feature should be active based on scroll progress
			const newIndex = Math.min(
				features.length - 1,
				Math.floor(scrollProgress * features.length)
			);

			if (newIndex !== activeFeatureIndex) {
				activeFeatureIndex = newIndex;
			}
		};

		window.addEventListener('scroll', handleScroll);
		handleScroll(); // Initial call

		return () => {
			window.removeEventListener('scroll', handleScroll);
		};
	});
</script>

<section bind:this={sectionElement} class="sticky-features-section" {...rest}>
	<div class="section-px container mx-auto">
		<div class="section-pb-sm container-sm grid text-balance text-title2">
			<SectionHeader {title} {subtitle} />
		</div>

		<div class="sticky-features-container">
			<!-- Left side: Feature list -->
			<div class="features-list">
				{#each features as feature, index}
					<div 
						class="feature-item" 
						class:active={index === activeFeatureIndex}
					>
						<h3 class="text-title3 mb-3">{feature.title}</h3>
						<p class="text-body opacity-60 max-w-prose">{feature.description}</p>
					</div>
				{/each}
			</div>

			<!-- Right side: Animated metaphors -->
			<div class="animation-container">
				<div class="animation-viewport">
					{#each metaphors as metaphor, index}
						<div 
							class="metaphor-animation {metaphor.animation}" 
							class:active={index === activeFeatureIndex}
						>
							<div class="metaphor-content">
								<div class="metaphor-visual">
									{#if metaphor.animation === 'streams'}
										<div class="stream stream-1"></div>
										<div class="stream stream-2"></div>
										<div class="stream stream-3"></div>
										<div class="convergence"></div>
									{:else if metaphor.animation === 'bridge'}
										<div class="island island-left"></div>
										<div class="island island-right"></div>
										<div class="bridge-arc"></div>
									{:else if metaphor.animation === 'constellation'}
										<div class="star star-1"></div>
										<div class="star star-2"></div>
										<div class="star star-3"></div>
										<div class="star star-4"></div>
										<div class="star star-5"></div>
										<div class="connection-line line-1"></div>
										<div class="connection-line line-2"></div>
										<div class="connection-line line-3"></div>
									{:else if metaphor.animation === 'clouds'}
										<div class="cloud cloud-1"></div>
										<div class="cloud cloud-2"></div>
										<div class="cloud cloud-3"></div>
									{:else if metaphor.animation === 'crystal'}
										<div class="crystal-node node-1"></div>
										<div class="crystal-node node-2"></div>
										<div class="crystal-node node-3"></div>
										<div class="crystal-node node-4"></div>
										<div class="crystal-edge edge-1"></div>
										<div class="crystal-edge edge-2"></div>
										<div class="crystal-edge edge-3"></div>
									{:else if metaphor.animation === 'morph'}
										<div class="morphing-shape"></div>
									{/if}
								</div>
								<div class="metaphor-text">
									<h4 class="text-title4 mb-2">{metaphor.title}</h4>
									<p class="text-body-sm opacity-70">{metaphor.description}</p>
								</div>
							</div>
						</div>
					{/each}
				</div>
			</div>
		</div>
	</div>
</section>

<!--
@component
A sticky-scrolling feature section with abstract metaphor animations.
Features are displayed on the left, with changing animations on the right as you scroll.

Usage:
```html
<Features
  title="Section Title"
  subtitle="Section Subtitle"
  features={[
    {
      title: "Feature Title",
      description: "Feature description text"
    }
    // more features...
  ]}
/>
```
-->

<style>
	.sticky-features-section {
		min-height: 300vh; /* Make it tall enough for sticky scrolling */
		padding: 4rem 0;
	}

	.sticky-features-container {
		display: grid;
		grid-template-columns: 1fr 1fr;
		gap: 4rem;
		align-items: start;
	}

	.features-list {
		position: sticky;
		top: 20vh;
		height: fit-content;
	}

	.feature-item {
		padding: 2rem 0;
		opacity: 0.3;
		transition: opacity 0.6s ease;
		border-bottom: 1px solid hsl(var(--border));
	}

	.feature-item:last-child {
		border-bottom: none;
	}

	.feature-item.active {
		opacity: 1;
	}

	.animation-container {
		position: sticky;
		top: 20vh;
		height: 60vh;
		display: flex;
		align-items: center;
		justify-content: center;
	}

	.animation-viewport {
		position: relative;
		width: 100%;
		height: 100%;
		max-width: 500px;
		max-height: 500px;
	}

	.metaphor-animation {
		position: absolute;
		top: 0;
		left: 0;
		width: 100%;
		height: 100%;
		opacity: 0;
		transition: opacity 0.8s ease;
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
	}

	.metaphor-animation.active {
		opacity: 1;
	}

	.metaphor-content {
		text-align: center;
		width: 100%;
	}

	.metaphor-visual {
		position: relative;
		width: 300px;
		height: 300px;
		margin: 0 auto 2rem;
	}

	.metaphor-text {
		margin-top: 2rem;
	}

	/* Streams Animation */
	.streams .stream {
		position: absolute;
		width: 4px;
		height: 120px;
		background: linear-gradient(180deg, transparent, hsl(var(--primary)), transparent);
		border-radius: 2px;
		animation: flow 3s ease-in-out infinite;
	}

	.streams .stream-1 {
		left: 50px;
		top: 20px;
		animation-delay: 0s;
	}

	.streams .stream-2 {
		left: 150px;
		top: 40px;
		animation-delay: 0.5s;
	}

	.streams .stream-3 {
		right: 50px;
		top: 30px;
		animation-delay: 1s;
	}

	.streams .convergence {
		position: absolute;
		bottom: 50px;
		left: 50%;
		transform: translateX(-50%);
		width: 80px;
		height: 80px;
		border-radius: 50%;
		background: radial-gradient(circle, hsl(var(--primary)), transparent);
		animation: pulse 2s ease-in-out infinite;
	}

	@keyframes flow {
		0%, 100% { transform: translateY(0) scale(1); opacity: 0.6; }
		50% { transform: translateY(20px) scale(1.1); opacity: 1; }
	}

	/* Bridge Animation */
	.bridge .island {
		position: absolute;
		width: 60px;
		height: 40px;
		background: hsl(var(--muted));
		border-radius: 50%;
		top: 50%;
		transform: translateY(-50%);
	}

	.bridge .island-left {
		left: 20px;
	}

	.bridge .island-right {
		right: 20px;
	}

	.bridge .bridge-arc {
		position: absolute;
		top: 50%;
		left: 50%;
		transform: translate(-50%, -50%);
		width: 200px;
		height: 100px;
		border: 3px solid hsl(var(--primary));
		border-bottom: none;
		border-radius: 100px 100px 0 0;
		animation: buildBridge 4s ease-in-out infinite;
	}

	@keyframes buildBridge {
		0% { transform: translate(-50%, -50%) scaleX(0); }
		50% { transform: translate(-50%, -50%) scaleX(1); }
		100% { transform: translate(-50%, -50%) scaleX(1); }
	}

	/* Constellation Animation */
	.constellation .star {
		position: absolute;
		width: 8px;
		height: 8px;
		background: hsl(var(--primary));
		border-radius: 50%;
		animation: twinkle 2s ease-in-out infinite;
	}

	.constellation .star-1 { top: 50px; left: 100px; animation-delay: 0s; }
	.constellation .star-2 { top: 80px; right: 80px; animation-delay: 0.4s; }
	.constellation .star-3 { bottom: 100px; left: 80px; animation-delay: 0.8s; }
	.constellation .star-4 { bottom: 80px; right: 100px; animation-delay: 1.2s; }
	.constellation .star-5 { top: 150px; left: 50%; transform: translateX(-50%); animation-delay: 1.6s; }

	.constellation .connection-line {
		position: absolute;
		height: 2px;
		background: linear-gradient(90deg, transparent, hsl(var(--primary)), transparent);
		animation: drawLine 3s ease-in-out infinite;
	}

	.constellation .line-1 {
		top: 54px;
		left: 108px;
		width: 80px;
		transform: rotate(15deg);
		animation-delay: 0.5s;
	}

	.constellation .line-2 {
		top: 154px;
		left: 58px;
		width: 60px;
		transform: rotate(-30deg);
		animation-delay: 1s;
	}

	.constellation .line-3 {
		bottom: 84px;
		right: 108px;
		width: 70px;
		transform: rotate(45deg);
		animation-delay: 1.5s;
	}

	@keyframes twinkle {
		0%, 100% { opacity: 0.4; transform: scale(1); }
		50% { opacity: 1; transform: scale(1.2); }
	}

	@keyframes drawLine {
		0% { transform: scaleX(0); opacity: 0; }
		50% { transform: scaleX(1); opacity: 1; }
		100% { transform: scaleX(1); opacity: 0.6; }
	}

	/* Clouds Animation */
	.clouds .cloud {
		position: absolute;
		background: hsl(var(--muted));
		border-radius: 50px;
		animation: float 6s ease-in-out infinite;
	}

	.clouds .cloud::before,
	.clouds .cloud::after {
		content: '';
		position: absolute;
		background: hsl(var(--muted));
		border-radius: 50px;
	}

	.clouds .cloud-1 {
		width: 80px;
		height: 40px;
		top: 60px;
		left: 50px;
		animation-delay: 0s;
	}

	.clouds .cloud-1::before {
		width: 50px;
		height: 50px;
		top: -25px;
		left: 10px;
	}

	.clouds .cloud-1::after {
		width: 60px;
		height: 40px;
		top: -15px;
		right: 10px;
	}

	.clouds .cloud-2 {
		width: 60px;
		height: 30px;
		top: 150px;
		right: 60px;
		animation-delay: 2s;
	}

	.clouds .cloud-2::before {
		width: 40px;
		height: 40px;
		top: -20px;
		left: 5px;
	}

	.clouds .cloud-2::after {
		width: 50px;
		height: 30px;
		top: -10px;
		right: 5px;
	}

	.clouds .cloud-3 {
		width: 70px;
		height: 35px;
		bottom: 80px;
		left: 50%;
		transform: translateX(-50%);
		animation-delay: 4s;
	}

	.clouds .cloud-3::before {
		width: 45px;
		height: 45px;
		top: -22px;
		left: 8px;
	}

	.clouds .cloud-3::after {
		width: 55px;
		height: 35px;
		top: -12px;
		right: 8px;
	}

	@keyframes float {
		0%, 100% { transform: translateY(0) translateX(0); }
		33% { transform: translateY(-10px) translateX(5px); }
		66% { transform: translateY(5px) translateX(-3px); }
	}

	/* Crystal Animation */
	.crystal .crystal-node {
		position: absolute;
		width: 12px;
		height: 12px;
		background: hsl(var(--primary));
		border-radius: 50%;
		animation: crystallize 3s ease-in-out infinite;
	}

	.crystal .node-1 { top: 80px; left: 80px; animation-delay: 0s; }
	.crystal .node-2 { top: 80px; right: 80px; animation-delay: 0.3s; }
	.crystal .node-3 { bottom: 80px; left: 80px; animation-delay: 0.6s; }
	.crystal .node-4 { bottom: 80px; right: 80px; animation-delay: 0.9s; }

	.crystal .crystal-edge {
		position: absolute;
		height: 2px;
		background: hsl(var(--primary));
		animation: growEdge 3s ease-in-out infinite;
	}

	.crystal .edge-1 {
		top: 86px;
		left: 92px;
		width: 126px;
		animation-delay: 0.5s;
	}

	.crystal .edge-2 {
		top: 86px;
		left: 86px;
		width: 90px;
		transform: rotate(90deg);
		transform-origin: left center;
		animation-delay: 0.8s;
	}

	.crystal .edge-3 {
		bottom: 86px;
		left: 92px;
		width: 126px;
		animation-delay: 1.1s;
	}

	@keyframes crystallize {
		0%, 100% { transform: scale(1); opacity: 0.7; }
		50% { transform: scale(1.3); opacity: 1; }
	}

	@keyframes growEdge {
		0% { transform: scaleX(0); opacity: 0; }
		50% { transform: scaleX(1); opacity: 1; }
		100% { transform: scaleX(1); opacity: 0.7; }
	}

	/* Morph Animation */
	.morph .morphing-shape {
		position: absolute;
		top: 50%;
		left: 50%;
		transform: translate(-50%, -50%);
		width: 100px;
		height: 100px;
		background: linear-gradient(45deg, hsl(var(--primary)), hsl(var(--secondary)));
		animation: morph 4s ease-in-out infinite;
	}

	@keyframes morph {
		0% { 
			border-radius: 50%;
			transform: translate(-50%, -50%) rotate(0deg) scale(1);
		}
		25% { 
			border-radius: 0;
			transform: translate(-50%, -50%) rotate(45deg) scale(1.1);
		}
		50% { 
			border-radius: 50% 0 50% 0;
			transform: translate(-50%, -50%) rotate(90deg) scale(0.9);
		}
		75% { 
			border-radius: 0 50% 0 50%;
			transform: translate(-50%, -50%) rotate(135deg) scale(1.2);
		}
		100% { 
			border-radius: 50%;
			transform: translate(-50%, -50%) rotate(180deg) scale(1);
		}
	}

	/* Responsive Design */
	@media (max-width: 768px) {
		.sticky-features-container {
			grid-template-columns: 1fr;
			gap: 2rem;
		}

		.features-list {
			position: static;
		}

		.animation-container {
			position: static;
			height: 400px;
		}

		.metaphor-visual {
			width: 250px;
			height: 250px;
		}

		.sticky-features-section {
			min-height: auto;
		}
	}

	@keyframes pulse {
		0%, 100% { transform: translateX(-50%) scale(1); opacity: 0.6; }
		50% { transform: translateX(-50%) scale(1.2); opacity: 1; }
	}
</style>
