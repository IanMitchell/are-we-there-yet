<script>
    import { session } from '$app/stores';
    import CheckCircle from '$components/CheckCircle.svelte';
    import Exclamation from '$components/Exclamation.svelte';
    import Show from '$components/Show.svelte';
    import { byLatestEpisode } from '$components/utils';
    import { onMount } from 'svelte';

    let divHeight;

    function messageParentAboutResize() {
        if (typeof window !== 'undefined') {
            window.parent.postMessage(
                JSON.stringify({
                    action: 'resize',
                    height: document.body.scrollHeight + 4,
                }),
                '*'
            );
        }
    }

    onMount(() => {
        messageParentAboutResize();
    });

    $: incompleteShows = ($session.shows || [])
        .filter((show) => show.progress !== 'Complete')
        .sort(byLatestEpisode);
    $: divHeight, messageParentAboutResize();
</script>

<div
    class="grid sm:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-2 px-1 sm:px-2 sm:py-2 bg-transparent"
    bind:clientWidth={divHeight}
>
    {#each incompleteShows as show (show.id)}
        <Show {show} />
    {:else}
        {#if $session.message}
            <!-- Error -->
            <div
                class="bg-white shadow pt-2 pb-1 px-3 rounded-t-none rounded-b-md flex flex-row border-t-2 border-yellow-500"
            >
                <div class="h-12 w-12 flex-none text-yellow-500">
                    <Exclamation />
                </div>
                <div class="px-3">
                    <h1 class="text-gray-700">Connection error</h1>
                    <h3 class="text-xs text-gray-500">{$session.message}</h3>
                </div>
            </div>
        {:else}
            <!-- No incomplete shows -->
            <div
                class="bg-white shadow pt-2 pb-1 px-3 rounded-t-none rounded-b-md flex flex-row border-t-2 border-green-500"
            >
                <div class="h-12 w-12 flex-none text-green-500">
                    <CheckCircle />
                </div>
                <div class="px-3">
                    <h1 class="text-gray-700">100%</h1>
                    <h3 class="text-xs text-gray-500">
                        All shows are complete!
                    </h3>
                </div>
            </div>
        {/if}
    {/each}
</div>
