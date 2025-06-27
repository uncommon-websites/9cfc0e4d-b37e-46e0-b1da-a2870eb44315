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

	const typeColors = {
		success: 'bg-green-50 border-green-200 text-green-800',
		info: 'bg-blue-50 border-blue-200 text-blue-800',
		warning: 'bg-yellow-50 border-yellow-200 text-yellow-800',
		error: 'bg-red-50 border-red-200 text-red-800'
	};

	const iconColors = {
		success: 'bg-green-100 text-green-600',
		info: 'bg-blue-100 text-blue-600',
		warning: 'bg-yellow-100 text-yellow-600',
		error: 'bg-red-100 text-red-600'
	};
</script>

<div class="notification-card rounded-xl border p-6 shadow-sm transition-all duration-300 hover:shadow-md {typeColors[type]}">
	<div class="flex items-start gap-4">
		<!-- Icon -->
		<div class="flex h-10 w-10 items-center justify-center rounded-full text-sm font-semibold {iconColors[type]}">
			{icons[type]}
		</div>

		<!-- Content -->
		<div class="flex-1 min-w-0">
			<div class="flex items-start justify-between gap-4">
				<div class="flex-1">
					<h3 class="font-semibold text-sm leading-tight mb-1">
						{title}
					</h3>
					<p class="text-sm opacity-80 leading-relaxed">
						{message}
					</p>
				</div>

				{#if showAvatar}
					<div class="flex-shrink-0">
						<img
							src={avatarSrc}
							alt="User avatar"
							class="h-8 w-8 rounded-full object-cover ring-2 ring-white"
						/>
					</div>
				{/if}
			</div>

			<!-- Footer -->
			<div class="flex items-center justify-between mt-4 pt-3 border-t border-current/10">
				<span class="text-xs opacity-60">
					{timestamp}
				</span>

				{#if actionLabel && onAction}
					<button
						onclick={onAction}
						class="text-xs font-medium hover:underline focus:outline-none focus:underline transition-all duration-200"
					>
						{actionLabel}
					</button>
				{/if}
			</div>
		</div>
	</div>
</div>