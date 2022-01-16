<script lang="ts">
    import Spinner from '../Spinner.svelte';
    import {csv} from 'd3';
    import {onMount} from "svelte";

    export let circleRadius = 8;
    export let quantityKey: string;
    export let categoryKey: string;
    export let width = 500;
    export let rowHeight = 50;
    export let color = 'red';
    export let opacity = 0.1;
    export let categoryLabelWidth = 0;
    export let categoryLabelPadding = 8;
    export let quantityMin: number;
    export let quantityMax: number;
    export let categoryLabelPaddingLeft = categoryLabelPadding;
    export let categoryLabelPaddingRight = categoryLabelPadding;
    export let meanPaddingLeft = categoryLabelPadding;
    export let paddingTop = 50;
    export let numberOfIntervals = 5;
    export let url: string;

    const labelOffSetY = circleRadius / 2 + 1;

    interface Data {
        [quantityKey]: number;
        [categoryKey]: string;
    }

    let fetchedData: [Data] = [];

    async function fetchData() {
        return csv(url, (d) => {
            fetchedData.push({
                [quantityKey]: d[quantityKey] as number,
                [categoryKey]: d[categoryKey] as string
            } as Data)
        })
    }

    onMount(async () => {
        await fetchData();
    })
    console.log(fetchedData)

</script>

{#await fetchedData}
    <Spinner/>
{:then fetchedData}

{/await}
