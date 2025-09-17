<script>
    import { syllabary } from './syllabary.js';
    import { selection, search } from './data.svelte.js';
    const { type, id, label } = $props();
    const other = type == 'initial' ? 'final' : 'initial';

    const available = $derived(
        selection[other] == undefined
        || syllabary.find(syl => {
            return (syl[type] == id && syl[other] == selection[other]);
        }) !== undefined
    );
</script>

<label class:shadow={!available}>
    {label}
    <input 
        type="radio"
        value={id}
        bind:group={selection[type]}
        onclick={ () => {
            search.value = '';
            if (selection[type] == id) {
                selection[type] = undefined;
            } else if (!available) {
                selection[other] = undefined;
            }
            if (type == "final" && id < 0) selection.initial = -1;
        } }
    />
</label>

<style>
    input { display: none; }
    label {
        display: flex;
        justify-content: center;
        align-items: center;
        &:not(:has(:checked)):hover {
            box-sizing: border-box;
            border: var(--border);
        }
        &:has(:checked) {
            border: var(--bold-border);
        }
        &.shadow {
            font-weight: var(--light); 
            color: var(--grey);
        }
    }
</style>