<script>
    import Position from '$components/Position.svelte';
    import { allMarked, hasAired, timeAgo } from '$components/utils';
    import InformationCircleSmall from './InformationCircleSmall.svelte';

    export let episode;
    export let status;
</script>

{#if episode}
    <div class="flex gap-1 flex-wrap items-center text-sm text-gray-800 pt-1">
        <span class="whitespace-nowrap">Ep. {episode.number}</span>
        {#if hasAired(episode)}
            {#if allMarked(episode)}
                <span>waiting for release</span>
            {:else}
                <span>requires</span>
                <div class="flex gap-1">
                    {#each episode.staff as staff (staff.id)}
                        {#if !staff.finished}
                            <Position position={staff.position} />
                        {/if}
                    {/each}
                </div>
            {/if}
        {:else}
            <span>
                Airs on
                <time datetime={episode.air_date}>
                    {new Date(episode.air_date).toLocaleString()}
                </time>
            </span>
        {/if}
    </div>
    <div class="absolute bottom-2 left-3 text-xs text-gray-400">
        <div class="flex flex-row gap-1 text-left">
            {#if status}
                <!-- Status Tooltip -->
                <span class="inline-block h-4 w-4 relative text-blue-400 group">
                    <InformationCircleSmall />
                    <span
                        class="pointer-events-none opacity-0 group-hover:opacity-100 transition-opacity shadow rounded rounded-bl-none absolute bottom-5 w-60 border bg-blue-100 text-blue-800 border-blue-400 px-2 py-2"
                    >
                        {status}
                    </span>
                </span>
            {/if}
            <span>
                Updated {timeAgo.format(new Date(episode.updated_at))}
            </span>
        </div>
    </div>
    <div class="absolute bottom-2 right-3 text-xs text-gray-400">
        <div class="flex flex-row text-right">
            <span>{episode.season}</span>
        </div>
    </div>
{/if}
