<script lang="ts">
	// Types
	type Testimonial = {
		name: string;
		position: string;
		company: string;
		quote: string;
		image: string; // a 9/16 portrait image of a person
	};

	// Props
	let { testimonials, ...rest }: { testimonials: Testimonial[] } = $props();

	// State
	let current = $state(0);
	let carouselRef: HTMLElement;
	let isDragging = $state(false);
	let startX = $state(0);
	let currentTranslate = $state(0);
	let prevTranslate = $state(0);
	let animationId = $state(0);

	import { onMount } from "svelte";

	// Navigation functions
	const goToSlide = (index: number) => {
		current = Math.max(0, Math.min(index, testimonials.length - 1));
		setSliderPosition();
	};

	const nextSlide = () => {
		if (current < testimonials.length - 1) {
			current++;
			setSliderPosition();
		}
	};

	const prevSlide = () => {
		if (current > 0) {
			current--;
			setSliderPosition();
		}
	};

	const setSliderPosition = () => {
		if (!carouselRef) return;
		const slideWidth = carouselRef.offsetWidth;
		currentTranslate = current * -slideWidth;
		prevTranslate = currentTranslate;
		setTransform();
	};

	const setTransform = () => {
		if (!carouselRef) return;
		carouselRef.style.transform = `translateX(${currentTranslate}px)`;
	};

	// Touch/Mouse event handlers
	const getPositionX = (event: TouchEvent | MouseEvent) => {
		return 'touches' in event ? event.touches[0].clientX : event.clientX;
	};

	const dragStart = (event: TouchEvent | MouseEvent) => {
		if ('touches' in event) {
			startX = event.touches[0].clientX;
		} else {
			startX = event.clientX;
			event.preventDefault();
		}
		isDragging = true;
		animationId = requestAnimationFrame(animation);
	};

	const dragMove = (event: TouchEvent | MouseEvent) => {
		if (!isDragging) return;
		
		const currentPosition = getPositionX(event);
		const diff = currentPosition - startX;
		currentTranslate = prevTranslate + diff;
	};

	const dragEnd = () => {
		isDragging = false;
		cancelAnimationFrame(animationId);

		const slideWidth = carouselRef?.offsetWidth || 0;
		const movedBy = currentTranslate - prevTranslate;

		// Determine if we should move to next/prev slide
		if (movedBy < -slideWidth / 4 && current < testimonials.length - 1) {
			current++;
		} else if (movedBy > slideWidth / 4 && current > 0) {
			current--;
		}

		setSliderPosition();
	};

	const animation = () => {
		setTransform();
		if (isDragging) {
			requestAnimationFrame(animation);
		}
	};

	// Keyboard navigation
	const handleKeydown = (event: KeyboardEvent) => {
		if (event.key === 'ArrowLeft') {
			prevSlide();
		} else if (event.key === 'ArrowRight') {
			nextSlide();
		}
	};

	onMount(() => {
		// Preload images
		testimonials.forEach((testimonial) => {
			if (testimonial.image) {
				const img = new Image();
				img.loading = "lazy";
				img.src = testimonial.image;
			}
		});

		// Set initial position
		setSliderPosition();

		// Auto-play functionality (optional)
		let autoPlayInterval: number;
		const startAutoPlay = () => {
			autoPlayInterval = setInterval(() => {
				if (current < testimonials.length - 1) {
					nextSlide();
				} else {
					goToSlide(0); // Loop back to start
				}
			}, 5000); // Change slide every 5 seconds
		};

		const stopAutoPlay = () => {
			clearInterval(autoPlayInterval);
		};

		// Start auto-play
		startAutoPlay();

		// Pause auto-play on hover/focus
		const carousel = carouselRef?.parentElement;
		carousel?.addEventListener('mouseenter', stopAutoPlay);
		carousel?.addEventListener('mouseleave', startAutoPlay);
		carousel?.addEventListener('focusin', stopAutoPlay);
		carousel?.addEventListener('focusout', startAutoPlay);

		// Handle resize
		const handleResize = () => {
			setSliderPosition();
		};

		window.addEventListener('resize', handleResize);

		return () => {
			clearInterval(autoPlayInterval);
			window.removeEventListener('resize', handleResize);
			carousel?.removeEventListener('mouseenter', stopAutoPlay);
			carousel?.removeEventListener('mouseleave', startAutoPlay);
			carousel?.removeEventListener('focusin', stopAutoPlay);
			carousel?.removeEventListener('focusout', startAutoPlay);
		};
	});
</script>

<section
	class="section-py section-px text-pretty [--gap:--spacing(4)]"
	{...rest}
>
	<div class="relative overflow-hidden">
		<!-- Carousel Container -->
		<div
			class="relative overflow-hidden rounded-xl"
			role="region"
			aria-label="Customer testimonials"
			tabindex="0"
			onkeydown={handleKeydown}
		>
			<div
				bind:this={carouselRef}
				class={[
					"flex transition-transform duration-300 ease-out",
					"[--inner-radius:calc(var(--outer-radius)-var(--gap))] [--outer-radius:var(--radius)] lg:[--outer-radius:var(--radius-xl)]"
				]}
				role="group"
				aria-label="Testimonials carousel"
				ontouchstart={dragStart}
				ontouchmove={dragMove}
				ontouchend={dragEnd}
				onmousedown={dragStart}
				onmousemove={dragMove}
				onmouseup={dragEnd}
				onmouseleave={dragEnd}
				style="user-select: none; -webkit-user-select: none;"
			>
				{#each testimonials as testimonial, index}
					<article
						class={[
							"lg:container-xs lg:grid-cols-[2fr_3fr]",
							"items-between grid grid-cols-1 gap-8",
							"bg-card dark:text-white",
							"aspect-video w-full flex-shrink-0 xl:aspect-[auto]",
							"rounded-(--outer-radius) p-(--gap)",
							"border-border border contain-layout"
						]}
						aria-hidden={current !== index}
					>
						<div class="hidden overflow-clip rounded-[max(var(--inner-radius),2px)] lg:block">
							{#if testimonial.image}
								<img
									src={testimonial.image}
									alt="{testimonial.name} testimonial"
									loading="lazy"
									class="aspect-[3/4] h-full w-full object-cover"
									draggable="false"
								/>
							{/if}
						</div>
						<div class="flex flex-col justify-between gap-12">
							<q class="text-title2 max-w-prose">{testimonial.quote}</q>
							<cite class="text-caption flex items-center gap-3 not-italic">
								{#if testimonial.image}
									<img
										src={testimonial.image}
										alt="{testimonial.name} testimonial"
										loading="lazy"
										class="size-12 rounded-full object-cover lg:hidden"
										draggable="false"
									/>
								{/if}
								<div>
									<p class="text-callout">{testimonial.name}</p>
									<p class="text-muted-foreground">
										{testimonial.position}, {testimonial.company}
									</p>
								</div>
							</cite>
						</div>
					</article>
				{/each}
			</div>
		</div>

		<!-- Navigation Arrows -->
		<button
			class="absolute left-4 top-1/2 -translate-y-1/2 rounded-full bg-white/90 p-2 shadow-lg transition-all hover:bg-white hover:scale-110 disabled:opacity-50 disabled:cursor-not-allowed dark:bg-gray-800/90 dark:hover:bg-gray-800"
			onclick={prevSlide}
			disabled={current === 0}
			aria-label="Previous testimonial"
		>
			<svg class="size-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
				<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 19l-7-7 7-7" />
			</svg>
		</button>

		<button
			class="absolute right-4 top-1/2 -translate-y-1/2 rounded-full bg-white/90 p-2 shadow-lg transition-all hover:bg-white hover:scale-110 disabled:opacity-50 disabled:cursor-not-allowed dark:bg-gray-800/90 dark:hover:bg-gray-800"
			onclick={nextSlide}
			disabled={current === testimonials.length - 1}
			aria-label="Next testimonial"
		>
			<svg class="size-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
				<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5l7 7-7 7" />
			</svg>
		</button>

		<!-- Pagination Indicators -->
		<div class="mt-8 flex justify-center gap-2">
			{#each testimonials as _, index}
				<button
					class="size-3 rounded-full transition-all duration-300 ease-in-out focus:outline-none focus:ring-2 focus:ring-primary-500 focus:ring-offset-2"
					class:bg-primary-600={current === index}
					class:bg-gray-300={current !== index}
					class:dark:bg-gray-600={current !== index}
					onclick={() => goToSlide(index)}
					aria-label="Go to testimonial {index + 1}"
				></button>
			{/each}
		</div>

		<!-- Progress Bar -->
		<div class="mt-4 mx-auto max-w-xs">
			<div class="h-1 bg-gray-200 rounded-full dark:bg-gray-700">
				<div
					class="h-1 bg-primary-600 rounded-full transition-all duration-300 ease-out"
					style="width: {((current + 1) / testimonials.length) * 100}%"
				></div>
			</div>
		</div>
	</div>
</section>

<style>
	/* Hide scrollbar while preserving functionality */
	.hide-scrollbar {
		-ms-overflow-style: none;
		scrollbar-width: none;
	}
	.hide-scrollbar::-webkit-scrollbar {
		display: none;
	}
</style>
