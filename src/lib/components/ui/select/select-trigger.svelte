<script lang="ts">
	import { Select as SelectPrimitive } from "bits-ui";
	import ChevronUp from "lucide-svelte/icons/chevron-up";
	import { cn } from "$lib/utils.js";

	type $$Props = SelectPrimitive.TriggerProps;
	type $$Events = SelectPrimitive.TriggerEvents;

	let className: $$Props["class"] = undefined;
	export { className as class };
</script>

<SelectPrimitive.Trigger
	class={cn(
		"border-2 border-black focus-visible:ring-ring aria-[invalid]:border-destructive flex h-10 w-full items-center justify-between px-3 py-2 text-sm focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-offset-2 disabled:cursor-not-allowed disabled:opacity-50 [&>span]:line-clamp-1",
		className
	)}
	{...$$restProps}
	let:builder
	on:click
	on:keydown
>
	<slot {builder} />
	<div>
		<ChevronUp class="h-4 w-4" />
	</div>
</SelectPrimitive.Trigger>
