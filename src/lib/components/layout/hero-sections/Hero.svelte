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

<div class="hero-container" {...rest}>
	<!-- Content -->
	<header
		class="hero-content section-px relative z-10 container mx-auto grid items-center gap-16 gap-y-12 py-20 text-balance {centered ? 'place-items-center text-center' : 'xl:grid-cols-[1fr_1fr]'}"
		data-enter-container
	>
		<div class="grid gap-8" class:max-w-prose={centered}>
			<h1 class="text-display hero-title w-full" data-enter>
				<span class="block"><AnimateText text={title} /></span>
			</h1>

			<p
				data-enter
				class="text-muted-foreground text-headline block max-w-[50ch] transition duration-500 ease-out {centered ? 'mx-auto' : ''}"
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

		{#if !centered}
			<div class="hero-notification-container" data-enter>
				<div class="grid gap-4 max-w-sm">
					<Notification
						title="Design review completed"
						message="Your prototype has been approved by the team. Ready to move to development phase."
						type="success"
						timestamp="2 minutes ago"
						actionLabel="View details"
					/>
					<Notification
						title="New comment on wireframe"
						message="Sarah added feedback on the checkout flow. Review suggested changes before the next sprint."
						type="info"
						timestamp="5 minutes ago"
						actionLabel="Reply"
					/>
					<Notification
						title="Component library updated"
						message="New button variants and form components are now available in your design system."
						type="info"
						timestamp="1 hour ago"
						actionLabel="Explore"
					/>
				</div>
			</div>
		{/if}
	</header>
</div>
