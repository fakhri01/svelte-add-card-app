<script>
    import { fly } from "svelte/transition";

    let visible = true;

    const hideNotification = () => {
        visible = false;
    };
</script>

{#if visible}
    <div
        transition:fly={{ y: -200, duration: 1000 }}
        class="notification-wrapper"
    >
        <p class="notification-message">
            You have succesfully added your card!
        </p>
        <button on:click={hideNotification} class="notification-close-btn"
            >X</button
        >
    </div>
{/if}

<style>
    .notification-wrapper {
        width: 350px;
        background-color: lightgreen;
        font-weight: 600;
        display: flex;
        justify-content: space-between;
        align-items: center;
        padding: 1rem;
        margin: 0 auto;
        position: absolute;
        right: 0;
        border-radius: 3px;
        transition: 0.5s all;
    }

    .notification-close-btn {
        border: none;
        background-color: transparent;
        cursor: pointer;
        font-weight: bold;
    }
</style>
