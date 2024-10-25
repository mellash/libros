<script lang="ts">
	import { goto } from '$app/navigation';

	import { Input } from '$lib/components/ui/input/index';

	import { Button } from '$lib/components/ui/button/index.js';
	import * as Select from '$lib/components/ui/select';

	import { toggleLoading } from '$store/globalStore';
	import { toast } from 'svelte-sonner';

	let searchQuery = '';
	let filterBy: string;

	type OnSubmitType = () => void;

	export let onSubmit: OnSubmitType | null = null;
	const handleSubmit = (e: SubmitEvent) => {
		e.preventDefault();

		if (onSubmit) {
			onSubmit();
		}

		if (!searchQuery || searchQuery.length < 3) {
			toast('Invalid input ');
			return;
		}
		toggleLoading();

		goto(`/search?query=${searchQuery}&filterBy=${filterBy || 'title'}&page=${1}`, {
			replaceState: true
		});
	};

	let options = ['Title', 'Author', 'Series', 'Publisher', 'Identifier', 'Tags'];
</script>

<div class="my-4 mx-6 flex w-full items-center justify-center">
	<form class="m-2 flex w-full items-center md:w-1/2" on:submit={handleSubmit}>
		<Select.Root
			onSelectedChange={(option) => {
				filterBy = String(option?.value);
			}}
		>
			<Select.Trigger class="w-[150px]">
				<Select.Value placeholder="Filter By" />
			</Select.Trigger>
			<Select.Content>
				{#each options as option}
					<Select.Item value={option}>{option}</Select.Item>
				{/each}
			</Select.Content>
		</Select.Root>
		<Input bind:value={searchQuery} type="search" placeholder="Search books" />
		<Button type="submit">
    <svg width="25" height="34" viewBox="0 0 25 34" fill="none" xmlns="http://www.w3.org/2000/svg">
<path d="M9.26221 21.5334L7.14834 27.4999" stroke="white" stroke-width="3.16495" stroke-linecap="square"/>
<path fill-rule="evenodd" clip-rule="evenodd" d="M12.5001 21.5335C16.6515 21.5335 20.0169 18.1681 20.0169 14.0167C20.0169 9.86536 16.6515 6.5 12.5001 6.5C8.34876 6.5 4.9834 9.86536 4.9834 14.0167C4.9834 18.1681 8.34876 21.5335 12.5001 21.5335ZM12.3352 18.2036C14.7386 18.2036 16.687 16.2552 16.687 13.8518C16.687 11.4484 14.7386 9.5 12.3352 9.5C9.93177 9.5 7.9834 11.4484 7.9834 13.8518C7.9834 16.2552 9.93177 18.2036 12.3352 18.2036Z" fill="white"/>
</svg>
    </Button>
	</form>
</div>
