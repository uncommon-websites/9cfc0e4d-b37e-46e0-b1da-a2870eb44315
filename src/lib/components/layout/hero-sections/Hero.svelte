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
	import Notification from "$lib/components/ui/Notification.svelte";

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
		centered = true,
		...rest
	}: Props = $props();
</script>

<div class="hero-container bg-background relative overflow-hidden" {...rest}>
	<!-- Content -->
	<header
		class="hero-content section-px section-py relative z-10 container mx-auto grid items-center gap-12 lg:gap-16 text-balance place-items-center text-center"
		data-enter-container
	>
		<!-- Notifications positioned above everything -->
		<div class="hero-notification-container w-full flex justify-center" data-enter>
			<div class="notification-stack max-w-sm lg:max-w-md">
				<div class="notification-item notification-1">
					<Notification
						title="Design review completed"
						message="Your prototype has been approved by the team. Ready to move to development phase."
						type="success"
						timestamp="2 minutes ago"
						actionLabel="View details"
					/>
				</div>
				<div class="notification-item notification-2">
					<Notification
						title="New comment on wireframe"
						message="Sarah added feedback on the checkout flow. Review suggested changes before the next sprint."
						type="info"
						timestamp="5 minutes ago"
						actionLabel="Reply"
					/>
				</div>
				<div class="notification-item notification-3">
					<Notification
						title="Component library updated"
						message="New button variants and form components are now available in your design system."
						type="info"
						timestamp="1 hour ago"
						actionLabel="Explore"
					/>
				</div>
			</div>
		</div>

		<div class="grid gap-6 lg:gap-8 max-w-prose">
			<h1 class="text-display hero-title w-full" data-enter>
				<span class="block"><AnimateText text={title} /></span>
			</h1>

			<p
				data-enter
				class="text-muted-foreground text-headline block max-w-prose text-pretty transition duration-500 ease-out mx-auto"
			>
				{subtitle}
			</p>

			{#if callsToAction.length > 0}
				<div class="flex flex-wrap gap-3 lg:gap-4 justify-center" data-enter>
					{#each callsToAction as cta, index}
						<Button
							href={cta.href}
							size="lg"
							variant={index % 2 === 0 ? "primary" : "secondary"}
							class="hero-button max-lg:hidden transition-transform hover:scale-105">{cta.label}</Button
						>
						<Button
							href={cta.href}
							size="md"
							variant={index % 2 === 0 ? "primary" : "secondary"}
							class="hero-button lg:hidden transition-transform hover:scale-105">{cta.label}</Button
						>
					{/each}
				</div>
			{/if}
		</div>
	</header>
</div>

<style>
	.notification-stack {
		position: relative;
		/* Responsive height that adapts to screen size */
		height: 180px;
		perspective: 1000px;
	}

	/* Mobile adjustments */
	@media (min-width: 640px) {
		.notification-stack {
			height: 200px;
		}
	}

	@media (min-width: 1024px) {
		.notification-stack {
			height: 240px;
		}
	}

	.notification-item {
		position: absolute;
		top: 0;
		left: 0;
		right: 0;
		width: 100%;
		opacity: 0;
		/* Responsive transform values */
		transform: translateY(20px) scale(0.9) rotateX(10deg);
		transition: all 0.8s cubic-bezier(0.4, 0, 0.2, 1);
		transform-origin: center bottom;
	}

	/* Desktop transform values */
	@media (min-width: 1024px) {
		.notification-item {
			transform: translateY(30px) scale(0.85) rotateX(15deg);
		}
	}

	.notification-1 {
		animation: enhancedNotificationLoop 12s infinite;
		animation-delay: 0s;
		z-index: 3;
	}

	.notification-2 {
		animation: enhancedNotificationLoop 12s infinite;
		animation-delay: 4s;
		z-index: 2;
	}

	.notification-3 {
		animation: enhancedNotificationLoop 12s infinite;
		animation-delay: 8s;
		z-index: 1;
	}

	/* Mobile-first keyframes with smaller movements */
	@keyframes enhancedNotificationLoop {
		0%, 8.33% {
			opacity: 0;
			transform: translateY(20px) scale(0.9) rotateX(10deg);
		}
		16.67%, 25% {
			opacity: 1;
			transform: translateY(0) scale(1) rotateX(0deg);
		}
		33.33%, 58.33% {
			opacity: 1;
			transform: translateY(0) scale(1) rotateX(0deg);
		}
		66.67%, 75% {
			opacity: 0.7;
			transform: translateY(-6px) scale(0.97) rotateX(-3deg);
		}
		83.33%, 100% {
			opacity: 0;
			transform: translateY(-25px) scale(0.85) rotateX(-10deg);
		}
	}

	/* Desktop keyframes with larger movements */
	@media (min-width: 1024px) {
		@keyframes enhancedNotificationLoop {
			0%, 8.33% {
				opacity: 0;
				transform: translateY(30px) scale(0.85) rotateX(15deg);
			}
			16.67%, 25% {
				opacity: 1;
				transform: translateY(0) scale(1) rotateX(0deg);
			}
			33.33%, 58.33% {
				opacity: 1;
				transform: translateY(0) scale(1) rotateX(0deg);
			}
			66.67%, 75% {
				opacity: 0.7;
				transform: translateY(-10px) scale(0.95) rotateX(-5deg);
			}
			83.33%, 100% {
				opacity: 0;
				transform: translateY(-40px) scale(0.8) rotateX(-15deg);
			}
		}
	}

	/* Responsive hover effects */
	.notification-stack:hover .notification-item {
		animation-play-state: paused;
		transform: translateY(0) scale(1.01) rotateX(0deg);
		opacity: 1;
	}

	.notification-stack:hover .notification-1 {
		transform: translateY(-3px) scale(1.01) rotateX(0deg);
	}

	.notification-stack:hover .notification-2 {
		transform: translateY(0) scale(1) rotateX(0deg);
		opacity: 0.8;
	}

	.notification-stack:hover .notification-3 {
		transform: translateY(3px) scale(0.99) rotateX(0deg);
		opacity: 0.6;
	}

	/* Desktop hover effects */
	@media (min-width: 1024px) {
		.notification-stack:hover .notification-item {
			transform: translateY(0) scale(1.02) rotateX(0deg);
		}

		.notification-stack:hover .notification-1 {
			transform: translateY(-5px) scale(1.02) rotateX(0deg);
		}

		.notification-stack:hover .notification-3 {
			transform: translateY(5px) scale(0.98) rotateX(0deg);
		}
	}

	/* Responsive floating animation */
	.notification-stack {
		animation: floatingStack 6s ease-in-out infinite;
	}

	@keyframes floatingStack {
		0%, 100% {
			transform: translateY(0px);
		}
		50% {
			transform: translateY(-4px);
		}
	}

	/* Desktop floating with more movement */
	@media (min-width: 1024px) {
		@keyframes floatingStack {
			0%, 100% {
				transform: translateY(0px);
			}
			50% {
				transform: translateY(-8px);
			}
		}
	}
</style>
