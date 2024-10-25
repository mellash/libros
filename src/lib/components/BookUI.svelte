<script lang="ts">
	// shadcdn components
	import type Book from '$lib/types/Book';
	import Button from './ui/button/button.svelte';
	import * as Dialog from '$lib/components/ui/dialog';

	// third-party
	import { CalendarDays } from 'lucide-svelte';
	import axios, { type AxiosProgressEvent } from 'axios';
	import { toast } from 'svelte-sonner';

	//custom components
	import DownloadProgress from './DownloadProgress.svelte';

	// misc
	import { API_SERVER_URL, NOT_AVAILABLE } from '$lib/constants';
	import { downloadBlob } from '$lib/utils';

	// stores
	import downloadStore, { addDownload, updateDownloadStatus } from '../../store/downloadStore';

	export let book: Book;

	const handleDownload = async (book: Book) => {
		try {
			addDownload(book);
			console.log('selected book', book);
			const res = await axios.post(`${API_SERVER_URL}/books/download`, book, {
				responseType: 'blob', // Set response type to blob
				onDownloadProgress: (progressEvent: AxiosProgressEvent) => {
					// this function gets called everytime the file gets updated with a new stream
					const total = progressEvent.total;
					const loaded = progressEvent.loaded;

					updateDownloadStatus(book, loaded, total || 0);
				}
			});

			if (res) {
				let blob = res.data;
				toast('Book downloaded successfully :)');
				downloadBlob(blob, $downloadStore.downloads.filter((b) => b.id === book.id)[0]);
			}
		} catch (error) {
			console.log(error);
		}
	};
</script>

<!-- Container -->
<div class="flex flex-col items-center border-r border-b border-t-0 [&:nth-child(4n)]:border-r-0 border-black">
  <!-- Book cover -->
	<div class="mt-4">
		<img
			class="h-[250px] w-[200px] object-cover"
			src={`${API_SERVER_URL}/proxy?url=${book.thumbUrl}`}
			alt="Book cover" 
      />
	</div>

  <!-- Book info -->
	<div class="flex flex-col items-start m-0 p-4 pb-0 md:h-52">
  <!-- Book title -->
		<div class="mb-2 flex items-center justify-between">
      <a href="#download-link">
        <p class="text-base font-bold leading-relaxed text-primary antialiased underline" style="text-decoration: underline">
          { book.title.replace(/\d{5,}/g, '').slice(0, 24)}
          { book.title.replace(/\d{5,}/g, '').length > 24 ?  '....' : ''}
        </p>
      </a>
		</div>

    <!-- Book author -->
		<div>
			<p class="text-slate-500 text-left">
				{#if book.authors.length}
						{book.authors.length > 100
							? `${book.authors.slice(0, 100)} ....`
							: book.authors}
				{:else}
					{book.authors.length && NOT_AVAILABLE}
				{/if}
			</p>
		</div>

	</div>
  <!-- Book info end -->
</div>
<!-- Container end -->
