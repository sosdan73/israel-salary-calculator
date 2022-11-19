<script>
    export let option1;
    export let option2;
    export let value;
    export let callback;

    $: clickHandler = option => {
        if (option !== value) {
            callback()
        }
    }
</script>

<div class="ToggleInput">
    <span
        class="ToggleInput__button"
        on:click={() => {clickHandler(true)}}
        class:--active={value}
    >
        {option1}
    </span>
    <span
        class="ToggleInput__button"
        on:click={() => {clickHandler(false)}}
        class:--active={!value}
    >
        {option2}
    </span>
</div>

<style>
    .ToggleInput {
        display: flex;
        width: 100%;
        border: 1px solid var(--dark);
        padding: 5px;
    }
    .ToggleInput__button {
        padding: 5px 10px;
        transition: background-color .3s ease-in-out;
        background-color: var(--light);
        cursor: default;
        width: 100%;
        text-align: center;
    }
    .ToggleInput__button:last-child {
        order: 2;
    }
    .ToggleInput:after {
        display: block;
        content: "";
        width: 0px;
        border-right: 1px solid var(--dark);
        order: 1;
        margin: 0 5px;
    }
    .ToggleInput__button:not(.--active):hover {
        background-color: var(--light-hover);
        cursor: pointer;
    }
    .ToggleInput__button.--active {
        background-color: var(--light-active);
    }
</style>