<script lang="ts">
	// svelte core
	import { onMount } from 'svelte';

	// shadcdn components
	import Button from '$lib/components/ui/button/button.svelte';
	import Badge from '$lib/components/ui/badge/badge.svelte';
	import * as Dialog from '$lib/components/ui/dialog';
	import * as Pagination from '$lib/components/ui/pagination';

	// custom components
	import SearchBar from '$lib/components/SearchBar.svelte';
	import DownloadProgress from '$lib/components/DownloadProgress.svelte';
	import BookUI from '$lib/components/BookUI.svelte';

	// custom types
	import type Book from '$lib/types/Book';

	// stores
	import downloadStore from '$store/downloadStore';
	import globalStore, { toggleLoading } from '../../store/globalStore';
	import BookSkeleton from '$lib/components/BookSkeleton.svelte';
	import { toast } from 'svelte-sonner';
	import Paginator from '$lib/components/Paginator.svelte';

	export let data;

	// state
	let searchResults: Book[];
	let query = '';
	let totalPages: number;
	let currentPage: number;
	let filterBy: string;
	let error: string;
	$: {
		if (data) {
			searchResults = data.searchResults;
			query = data.query || '';
			filterBy = data.filterBy!;
			totalPages = data.totalPages || 0;
			currentPage = parseInt(data.currentPage || '') || 1;
			if (data.error) {
				error = data.error;
				toast(error);
			}
			// this is usefull to stop loading after searching on the search page itself
			// which is here
			toggleLoading(false);
		}
	}

	onMount(() => {
		toggleLoading(false);
	});
</script>

<!-- Container -->
<div class="">
  <!-- Navigations start -->
	<div class="flex items-center justify-between ml-[10%] mr-[10%]">

    <!-- Home button -->
		<div class="flex items-center">
			<a href="/">
        <button>Home</button>
      </a>
		</div>

    <!-- Search bar -->
    <div class="flex grow">
      <SearchBar onSubmit={() => (searchResults = [])} />
    </div>

    <!-- Download button -->
    <Dialog.Root>
    <Dialog.Trigger on:click={() => {}}>
      <button class="flex items-center">
        {$downloadStore.downloads.length} Downloads 
        <svg style="margin-left: 4px;" width="12" height="12" viewBox="0 0 8 12" fill="none" xmlns="http://www.w3.org/2000/svg">
        <path d="M4 1.25V9.25M4 9.25L1 7.75M4 9.25L7 7.75M1 10.75H7" stroke="black" stroke-opacity="0.7" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
        </svg>
      </button>
    </Dialog.Trigger>
      <Dialog.Content>
        <DownloadProgress />
      </Dialog.Content>
    </Dialog.Root>
	</div>
  <!-- Navigation end -->

  <!-- Searched term -->
  <div class="w-full">
    <h2 class="text-[32px] border-black border-t-[1px] border-b-[1px] pl-[6%] pt-[12px] pb-[12px] mt-[30px] mb-[60px] w-full">{query.charAt(0).toUpperCase() + query.slice(1)}</h2>
  </div>

  <!-- No books -->
	{#if searchResults.length == 0 && !$globalStore.loading}
		<div class=" h-full w-full">
			<p class=" m-4 text-center text-3xl">Looks like we got no books for this search</p>
		</div>
	{/if}

	<div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-4 ml-[6%] mr-[6%]">
    <!-- Books -->
		{#each searchResults as book, i (i)}
			<BookUI {book} />
		{/each}

    <!-- Loading books -->
		{#if $globalStore.loading && searchResults.length < 1}
			<BookSkeleton />
			<BookSkeleton />
			<BookSkeleton />
			<BookSkeleton />
			<BookSkeleton />
			<BookSkeleton />
		{/if}
	</div>

  <!-- Paginations -->
  {#if totalPages && searchResults}
    <div class="border-black border-t-[1px] mt-[80px] mb-[40px]">
      <Paginator
        {totalPages}
        {currentPage}
        perPage={25}
        {query}
        {filterBy}
        onPreceed={() => (searchResults = [])} />
    </div>
  {/if}

</div>
<!-- Container end -->
