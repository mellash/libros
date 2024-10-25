<script lang="ts">
	import { Button as ButtonPrimitive } from "bits-ui";
	import { type Events, type Props, buttonVariants } from "./index.js";
	import { cn } from "$lib/utils.js";

	type $$Props = Props;
	type $$Events = Events;

	let className: $$Props["class"] = undefined;
	export let variant: $$Props["variant"] = "default";
	export let size: $$Props["size"] = "default";
	export let builders: $$Props["builders"] = [];
	export { className as class };
</script>

<ButtonPrimitive.Root
	{builders}
	class={cn('border-2 border-black w-12 h-10 bg-black text-white pl-2 pr-2')}
	type="button"
	{...$$restProps}
	on:click
	on:keydown
>
	<slot />
</ButtonPrimitive.Root>
