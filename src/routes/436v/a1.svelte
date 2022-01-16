<script lang="ts">
    import trailAccuracy from '../../assets/data/a1/experiment_data.csv';
    import ScatterPlot1D from '../../components/d3/ScatterPlot1D.svelte';
</script>

<h1>Trails</h1>
<ScatterPlot1D data={trailAccuracy} categoryKey="trial" quantityKey="accuracy"/>
