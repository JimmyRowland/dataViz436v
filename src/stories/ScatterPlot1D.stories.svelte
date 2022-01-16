<script lang="ts">
    import {Meta, Story, Template} from '@storybook/addon-svelte-csf';
    import ScatterPlot1DWithUrl from "../components/d3/ScatterPlot1DWithUrl.svelte";
</script>

<Meta title="Components/ScatterPlot1DWithUrl" component={ScatterPlot1DWithUrl}/>

<Template let:args>
    <ScatterPlot1DWithUrl {...args}/>
</Template>

<Story
        name="Primary"
        args={{
			categoryKey:"trial",
			quantityKey:"accuracy",
			url:"https://data-viz436v.vercel.app/data/a1/experiment_data.csv",
        }}
/>
