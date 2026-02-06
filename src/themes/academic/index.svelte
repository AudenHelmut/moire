<script lang="ts">
  import {format} from 'date-fns';
  import type {PageData} from '../../routes/$types';
  import {createMemoList} from '$lib/memo.svelte';

  let {data, config}: {data: PageData; config: any} = $props();
  const memoList = createMemoList(() => data, config);
</script>

<div class="max-w-4xl mx-auto min-h-screen bg-[var(--bg-color)] text-[var(--text-color)] p-5 md:p-10 {config.theme}">
  <div
    class="fixed inset-0 pointer-events-none opacity-40 bg-[url('data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIyMDAiIGhlaWdodD0iMjAwIiB2aWV3Qm94PSIwIDAgMTAwIDEwMCI+PHJlY3Qgd2lkdGg9IjEwMCUiIGhlaWdodD0iMTAwJSIgZmlsbD0iI2ZmZiIvPjxmaWx0ZXIgaWQ9Im4iPjxmZVR1cmJ1bGVuY2UgdHlwZT0iZnJhY3RhbE5vaXNlIiBiYXNlRnJlcXVlbmN5PSIwLjUiIG51bU9jdGF2ZXM9IjUiIHN0aXRjaFRpbGVzPSJzdGl0Y2giLz48L2ZpbHRlcj48cmVjdCB3aWR0aD0iMTAwJSIgaGVpZ2h0PSIxMDAlIiBmaWx0ZXI9InVybCgjbikiIG9wYWNpdHk9IjAuMSIvPjwvc3ZnPg==')] z-0"
  ></div>

  <div class="relative z-10 grid grid-cols-1 md:grid-cols-12 gap-12">
    <aside class="md:col-span-3 md:border-r border-[#333]/10 md:pr-8 md:text-right">
      <header class="sticky top-10">
        <h1 class="text-3xl font-bold italic mb-4 text-[var(--accent-color)]">{config.title}</h1>
        <div class="text-sm text-[#666] italic mb-8">
          <p>{config.description}</p>
          <p class="mt-4">— {config.author}</p>
        </div>

        {#if memoList.selectedTag}
          <div class="mb-8">
            <p class="text-xs font-bold uppercase tracking-wider text-[#999] mb-2">Filtered View</p>
            <button
              class="bg-[var(--accent-color)]/10 text-[var(--accent-color)] px-3 py-1 rounded-full text-sm italic hover:bg-[var(--accent-color)]/20 transition-colors"
              onclick={() => memoList.selectTag(null)}
            >
              #{memoList.selectedTag} ✕
            </button>
          </div>
        {/if}

        <div class="hidden md:block">
          <p class="text-xs font-bold uppercase tracking-wider text-[#999] mb-4">Index</p>
          <ul class="text-sm space-y-2">
            {#each memoList.allTags as tag}
              <li class="list-none">
                <button
                  class="hover:text-[var(--accent-color)] hover:underline decoration-[var(--accent-color)]/30 underline-offset-4 transition-colors {memoList.selectedTag ===
                  tag
                    ? 'text-[var(--accent-color)] font-bold'
                    : 'text-[#666]'}"
                  onclick={() => memoList.selectTag(tag)}
                >
                  #{tag}
                </button>
              </li>
            {/each}
          </ul>
        </div>
      </header>
    </aside>

    <main class="md:col-span-9 space-y-16 max-w-2xl">
      {#each Object.entries(memoList.groupedMemos) as [dateKey, memos]}
        <section>
          <div class="flex items-baseline gap-4 mb-8 border-b border-[var(--text-color)]/10 pb-2">
            <h2 class="text-2xl font-bold italic text-[var(--text-color)]">
              {format(new Date(dateKey + 'T00:00:00'), 'MMMM dd, yyyy')}
            </h2>
            <span class="text-sm text-[#999] italic">{format(new Date(dateKey + 'T00:00:00'), 'EEEE')}</span>
          </div>

          <div class="space-y-12">
            {#each memos as memo}
              <article class="group relative">
                <div class="absolute -left-16 top-1 hidden xl:block text-xs font-mono text-[#999] w-12 text-right">
                  {format(memo.date, 'HH:mm')}
                </div>

                <div class="xl:hidden text-xs font-mono text-[#999] mb-2">
                  {format(memo.date, 'HH:mm')}
                </div>

                <div
                  class="max-w-none text-[0.95rem] leading-relaxed
                        [&_h1]:text-[1.2rem] [&_h1]:font-normal [&_h1]:italic [&_h1]:mb-4 [&_h1]:mt-6
                        [&_h2]:text-[1.1rem] [&_h2]:font-normal [&_h2]:italic [&_h2]:mb-4 [&_h2]:mt-5
                        [&_h3]:text-[1.0rem] [&_h3]:font-normal [&_h3]:italic [&_h3]:mb-3 [&_h3]:mt-4
                        [&_h4]:text-[0.9rem] [&_h4]:font-normal [&_h4]:italic [&_h4]:mb-3 [&_h4]:mt-3
                        [&_h5]:text-[0.8rem] [&_h5]:font-normal [&_h5]:italic [&_h5]:mb-2 [&_h5]:mt-2
                        [&_p]:mb-4
                        [&_a]:text-[var(--accent-color)] [&_a]:no-underline [&_a]:border-b [&_a]:border-[var(--accent-color)]/30 [&_a]:underline-offset-2 [&_a]:hover:border-[var(--accent-color)] [&_a]:hover:bg-[var(--accent-color)]/5
                        [&_ul]:list-disc [&_ul]:pl-5 [&_ol]:list-decimal [&_ol]:pl-5
                        [&_table]:border-collapse [&_table]:border-y-2 [&_table]:border-black [&_table]:my-6 [&_table]:w-full [&_table]:text-xs
                        [&_th]:border-b-2 [&_th]:border-black [&_th]:border-dashed [&_th]:font-bold [&_th]:p-2 [&_th]:text-center [&_th]:uppercase
                        [&_td]:border-b [&_td]:border-[#ccc] [&_td]:border-dashed [&_td]:p-2 [&_td]:text-center
                        [&_blockquote]:border-l-4 [&_blockquote]:border-[var(--accent-color)] [&_blockquote]:bg-[#f9f2f4] [&_blockquote]:py-2 [&_blockquote]:px-4 [&_blockquote]:my-4 [&_blockquote]:not-italic [&_blockquote]:text-[0.9rem] [&_blockquote]:text-[#555]
                        [&_img]:shadow-lg [&_img]:rounded-sm [&_img]:border-[4px] [&_img]:border-white [&_img]:rotate-1 group-hover:[&_img]:rotate-0 [&_img]:transition-transform [&_img]:duration-500
                        [&_.tag-link]:text-xs [&_.tag-link]:text-[#999] [&_.tag-link:hover]:text-[var(--accent-color)] [&_.tag-link:hover]:underline [&_.tag-link]:no-underline"
                  onclick={(e) => {
                      const target = e.target as HTMLElement;
                      if (target.classList.contains('tag-link')) {
                          const tag = target.dataset.tag;
                          if (tag) memoList.selectTag(tag);
                      }
                  }}
                >
                  {@html memo.content}
                </div>
              </article>
            {/each}
          </div>
        </section>
      {/each}

      {#if memoList.visibleCount < data.memos.length}
        <div class="pt-12 text-center border-t border-[#333]/10">
          <button
            class="text-lg italic text-[#666] hover:text-[#d33682] transition-colors"
            onclick={memoList.loadMore}
          >
            ❧ Continue Reading...
          </button>
        </div>
      {/if}

      <footer class="mt-20 text-center text-sm text-[#999] italic pb-12">
        <p>— End of Manuscript —</p>
      </footer>
    </main>
  </div>
</div>

<style>
  :global(body) {
    font-variant-ligatures: common-ligatures;
  }
</style>
