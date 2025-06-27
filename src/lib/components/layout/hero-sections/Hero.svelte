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
				<img src={imageSrc} alt="Team collaboration" class="hero-image" loading="eager" />
			</div>
		{/if}
	</header>
</div>
