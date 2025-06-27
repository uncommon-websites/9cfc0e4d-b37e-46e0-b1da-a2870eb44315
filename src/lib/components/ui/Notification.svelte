<script lang="ts">
	// Types
	type Props = {
		title: string;
		message: string;
		type?: 'success' | 'info' | 'warning' | 'error';
		showAvatar?: boolean;
		avatarSrc?: string;
		timestamp?: string;
		actionLabel?: string;
		onAction?: () => void;
	};

	let {
		title,
		message,
		type = 'info',
		showAvatar = true,
		avatarSrc = '/generated/image-a-professional-headshot-of-a-modern-crea.webp',
		timestamp = 'Just now',
		actionLabel = 'View',
		onAction
	}: Props = $props();

	// Icon mapping for different notification types
	const icons = {
		success: '✓',
		info: 'i',
		warning: '⚠',
		error: '✕'
	};

	// Using design system colors instead of Tailwind classes
	const getTypeStyles = (type: string) => {
		switch (type) {
			case 'success':
				return {
					background: 'color-mix(in oklch, var(--color-secondary-100), var(--color-background) 50%)',
					border: 'var(--color-secondary-200)',
					text: 'var(--color-secondary-900)',
					iconBg: 'var(--color-secondary-200)',
					iconText: 'var(--color-secondary-700)'
				};
			case 'warning':
				return {
					background: 'color-mix(in oklch, var(--color-primary-100), var(--color-background) 50%)',
					border: 'var(--color-primary-200)',
					text: 'var(--color-primary-900)',
					iconBg: 'var(--color-primary-200)',
					iconText: 'var(--color-primary-700)'
				};
			case 'error':
				return {
					background: 'color-mix(in oklch, var(--color-destructive), var(--color-background) 80%)',
					border: 'color-mix(in oklch, var(--color-destructive), var(--color-background) 60%)',
					text: 'var(--color-foreground)',
					iconBg: 'color-mix(in oklch, var(--color-destructive), var(--color-background) 60%)',
					iconText: 'var(--color-foreground)'
				};
			default: // info
				return {
					background: 'var(--color-card)',
					border: 'var(--color-border)',
					text: 'var(--color-card-foreground)',
					iconBg: 'var(--color-accent)',
					iconText: 'var(--color-accent-foreground)'
				};
		}
	};

	let typeStyles = $derived(getTypeStyles(type));
</script>

<div class="notification-card border transition-all duration-300 w-full" style="border-radius: var(--radius-lg); padding: 1.5rem; background-color: {typeStyles.background}; border-color: {typeStyles.border}; color: {typeStyles.text};">
	<div class="flex items-start" style="gap: 1rem;">
		<!-- Icon -->
		<div class="flex items-center justify-center text-sm font-semibold" style="height: 2.5rem; width: 2.5rem; border-radius: 50%; background-color: {typeStyles.iconBg}; color: {typeStyles.iconText};">
			{icons[type]}
		</div>

		<!-- Content -->
		<div class="flex-1 min-w-0">
			<div class="flex items-start justify-between" style="gap: 1rem;">
				<div class="flex-1">
					<h3 class="text-headline font-semibold" style="margin-bottom: 0.25rem;">
						{title}
					</h3>
					<p class="text-body" style="color: var(--color-muted-foreground);">
						{message}
					</p>
				</div>

				{#if showAvatar}
					<div class="flex-shrink-0">
						<img
							src={avatarSrc}
							alt="User avatar"
							style="height: 2rem; width: 2rem; border-radius: 50%; object-fit: cover; border: 2px solid var(--color-background);"
						/>
					</div>
				{/if}
			</div>

			<!-- Footer -->
			<div class="flex items-center justify-between border-t" style="margin-top: 1rem; padding-top: 0.75rem; border-color: var(--color-border);">
				<span class="text-footnote">
					{timestamp}
				</span>

				{#if actionLabel && onAction}
					<button
						onclick={onAction}
						class="text-footnote font-medium hover:underline focus:outline-none focus:underline transition-all duration-200"
						style="color: var(--color-primary);"
					>
						{actionLabel}
					</button>
				{/if}
			</div>
		</div>
	</div>
</div>