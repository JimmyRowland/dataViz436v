<script lang="ts">
    import {extent, mean, rollup, scaleBand, scaleLinear} from 'd3';

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

    const labelOffSetY = circleRadius / 2 + 1;

    interface Data {
        [quantityKey]: number;
        [categoryKey]: string;
    }

    export let data: [Data];
    const fetchedData: [Data] = data || ([] as [Data]);

    const means = rollup(
        fetchedData,
        (fetchedData) => mean(fetchedData, (d) => d[quantityKey] as number),
        (d) => d[categoryKey] as string
    );
    const roundedMeans = Array.from(means.entries()).reduce((roundedMeans, [category, mean]) => {
        roundedMeans[category] = Math.round(mean * 100) / 100;
        return roundedMeans;
    }, {});

    function getLabelMaxWidth(labels: [string], prefix = '') {
        const maxLabelLength = Math.max(...labels.map((label) => label.length));
        const CHAR_WIDTH = 4;
        return (
            (prefix.length + maxLabelLength + 1) * CHAR_WIDTH +
            categoryLabelPaddingLeft +
            categoryLabelPaddingRight
        );
    }

    const leftMargin = categoryLabelWidth || getLabelMaxWidth(Array.from(means.keys()), categoryKey);
    const rightMargin = getLabelMaxWidth(Object.values(roundedMeans).map((mean) => mean.toString()));
    const height = means.size * rowHeight + paddingTop;
    const xExtent = extent(fetchedData, (d) => d[quantityKey] as number);
    const xMin = quantityMin ?? Math.floor(xExtent[0]);
    const xMax = quantityMax ?? Math.ceil(xExtent[1]);
    const xScale = scaleLinear()
        .domain([xMin, xMax])
        .range([
            leftMargin + categoryLabelPaddingLeft + categoryLabelPaddingRight + circleRadius,
            width - circleRadius - meanPaddingLeft - rightMargin
        ]);
    const yScale = scaleBand()
        .domain(Object.keys(roundedMeans).sort().reverse())
        .range([height + circleRadius, circleRadius + paddingTop]);

    const categoryIndexRoundedMeanMap = Object.keys(roundedMeans).reduce(
        (categoryIndexMap, category, index) => {
            categoryIndexMap[category] = {index, mean: roundedMeans[category], y: yScale(category)};
            return categoryIndexMap;
        },
        {} as { [category: string]: { mean: string; index: number; y: number } }
    );

    const correctedNumberOfIntervals = numberOfIntervals > 1 ? numberOfIntervals : 1;

    const verticalLines = xScale.ticks(correctedNumberOfIntervals + 1).map((tick) => {
        return {x: xScale(tick), label: tick.toFixed(1)};
    });
</script>

<svg {height} {width}>
    {#each fetchedData as data}
        <circle
                cx={xScale(data[quantityKey])}
                cy={categoryIndexRoundedMeanMap[data[categoryKey]].y}
                r={circleRadius}
                fill={color}
                {opacity}
        />
    {/each}
    {#each Object.entries(categoryIndexRoundedMeanMap) as [category, {mean, index, y}]}
        <text
                style="text-transform: capitalize"
                y={y + labelOffSetY}
                x={categoryLabelPaddingLeft}
                text-anchor="start">
            {`${categoryKey} ${category}`}
        </text>
        <text y={y + labelOffSetY} x={width} text-anchor="end">{mean}</text>
    {/each}
    {#each verticalLines as {x, label}}
        <line
                x1={x}
                x2={x}
                y1={paddingTop - circleRadius}
                y2={height - rowHeight + 3 * circleRadius}
                stroke="black"
                stroke-width="1"
                opacity={0.2}
        />
        <text {x} y={height} text-anchor="middle">{label}</text>
    {/each}

    <text y={15} x={width} text-anchor="end">
        {`${quantityKey[0].toUpperCase()}${quantityKey.slice(1, quantityKey.length)} (mean)`}
    </text>
</svg>
