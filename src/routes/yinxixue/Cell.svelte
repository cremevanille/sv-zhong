<script>
    import { selection, available, search } from './data.svelte.js';
    const { name, id, label } = $props();
</script>

<label
    class={ available(name, id) ? '' : 'shadow ' }
>
    {label}
    <input 
        type="radio"
        {name}
        value={id}
        bind:group={selection[name]}
        onclick={e => {
            search.value = '';
            if (selection[name] == id) {
                selection[name] = undefined;
            } else if (!available(name, id)) {
                const other = name == 'initial' ? 'final' : 'initial';
                selection[other] = undefined;
            }
        }}
        />
</label>

<style>
    input { display: none; }
    label {
        display: flex;
        justify-content: center;
        align-items: center;
    }
    label:not(:has(:checked)):hover {
        box-sizing: border-box;
        border: var(--border);
    }
    label:has(:checked) {
        border: var(--bold-border);
    }
    label.shadow {
        font-weight: var(--light); color: var(--grey);
    }
</style>