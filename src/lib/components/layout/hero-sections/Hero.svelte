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
		class="hero-content section-px section-py relative z-10 container mx-auto grid items-center gap-12 lg:gap-16 text-balance {centered ? 'place-items-center text-center' : 'xl:grid-cols-[1fr_1fr]'}"
		data-enter-container
	>
		<div class="grid gap-6 lg:gap-8" class:max-w-prose={centered}>
			<h1 class="text-display hero-title w-full" data-enter>
				<span class="block"><AnimateText text={title} /></span>
			</h1>

			<p
				data-enter
				class="text-muted-foreground text-headline block max-w-prose text-pretty transition duration-500 ease-out {centered ? 'mx-auto' : ''}"
			>
				{subtitle}
			</p>

			{#if callsToAction.length > 0}
				<div class="flex flex-wrap gap-3 lg:gap-4" data-enter class:justify-center={centered}>
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

		{#if !centered}
			<div class="hero-notification-container" data-enter>
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
		{/if}
	</header>
</div>

<style>
	.notification-stack {
		position: relative;
		height: 200px; /* Fixed height to contain stacked notifications */
	}

	.notification-item {
		position: absolute;
		top: 0;
		left: 0;
		right: 0;
		opacity: 0;
		transform: translateY(20px);
		transition: all 0.6s ease-in-out;
	}

	.notification-1 {
		animation: notificationLoop 9s infinite;
		animation-delay: 0s;
	}

	.notification-2 {
		animation: notificationLoop 9s infinite;
		animation-delay: 3s;
	}

	.notification-3 {
		animation: notificationLoop 9s infinite;
		animation-delay: 6s;
	}

	@keyframes notificationLoop {
		0%, 11.11% {
			opacity: 0;
			transform: translateY(20px);
		}
		22.22%, 77.78% {
			opacity: 1;
			transform: translateY(0);
		}
		88.89%, 100% {
			opacity: 0;
			transform: translateY(-20px);
		}
	}

	/* Pause animation on hover for better UX */
	.notification-stack:hover .notification-item {
		animation-play-state: paused;
	}
</style>
