<script lang="ts">
	import { Pagination as PaginationPrimitive } from "bits-ui";
	import ChevronLeft from "lucide-svelte/icons/chevron-left";
	import { Button } from "$lib/components/ui/button/index.js";
	import { cn } from "$lib/utils.js";

	type $$Props = PaginationPrimitive.PrevButtonProps;
	type $$Events = PaginationPrimitive.PrevButtonEvents;

	let className: $$Props["class"] = undefined;
	export { className as class };
</script>

<PaginationPrimitive.PrevButton asChild let:builder>
	<button
		class="flex items-center pr-[18px]"
		on:click
		{...$$restProps}
	>
		<slot>
        <svg width="12" height="8" viewBox="0 0 12 8" fill="none" xmlns="http://www.w3.org/2000/svg">
          <path d="M11.5 4.5H12V3.5H11.5V4.5ZM0.646447 3.64645C0.451184 3.84171 0.451184 4.15829 0.646447 4.35355L3.82843 7.53553C4.02369 7.7308 4.34027 7.7308 4.53553 7.53553C4.7308 7.34027 4.7308 7.02369 4.53553 6.82843L1.70711 4L4.53553 1.17157C4.7308 0.976311 4.7308 0.659728 4.53553 0.464466C4.34027 0.269204 4.02369 0.269204 3.82843 0.464466L0.646447 3.64645ZM11.5 3.5H1V4.5H11.5V3.5Z" fill="black"/>
      </svg>
			<span class="pl-[12px]">Previous</span>
		</slot>
	</button>
</PaginationPrimitive.PrevButton>
