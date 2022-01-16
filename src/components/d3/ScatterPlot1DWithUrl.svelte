<script lang="ts">
    import Spinner from '../Spinner.svelte';
    import {csv} from 'd3';
    import {onMount} from "svelte";
    import ScatterPlot1D from "./ScatterPlot1D.svelte";

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

    interface Data {
        [quantityKey]: number;
        [categoryKey]: string;
    }

    let fetchedData: [Data] = [];
    onMount(async () => {
        let data: [Data] = [];
        await csv(url, (d) => {
            data.push({
                [quantityKey]: +d[quantityKey],
                [categoryKey]: d[categoryKey]
            } as Data);
        })
        fetchedData = data
    })
</script>


{#if !fetchedData.length}
    <Spinner/>
{:else}
    <ScatterPlot1D
            data={fetchedData}
            quantityKey={quantityKey}
            categoryKey={categoryKey}
            width={width}
            rowHeight={rowHeight}
            color={color}
            opacity={opacity}
            categoryLabelWidth={categoryLabelWidth}
            categoryLabelPadding={categoryLabelPadding}
            categoryLabelPaddingLeft={categoryLabelPaddingLeft}
            categoryLabelPaddingRight={categoryLabelPaddingRight}
            meanPaddingLeft={meanPaddingLeft}
            paddingTop={paddingTop}
            numberOfIntervals={numberOfIntervals}
            quantityMin={quantityMin}
            quantityMax={quantityMax}
            circleRadius={circleRadius}
    />
{/if}
