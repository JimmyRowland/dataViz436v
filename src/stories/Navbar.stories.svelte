<script>
	import {Meta, Story, Template} from '@storybook/addon-svelte-csf';
	import Navbar from '../components/navbar.svelte';
</script>

<Meta title="Components/Navbar" component={Navbar}/>

<Template let:args>
	<Navbar {...args}/>
</Template>

<Story
		name="Primary"
		args={{
		primary: true,
		label: 'Button'
	}}
/>
