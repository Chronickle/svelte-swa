<script>
    import { onMount } from "svelte";
    import { Link } from "svelte-routing";

    const providers = ["x", "github", "aad"];
    const redirect = window.location.pathname;
    let userInfo = undefined;

    onMount(async () => (userInfo = await getUserInfo()));

    async function getUserInfo() {
        try {
            const response = await fetch("/.auth/me");
            const payload = await response.json();
            const { clientPrincipal } = payload;
            return clientPrincipal;
        } catch (error) {
            console.error("No profile could be found");
            return undefined;
        }
    }
</script>

<h1>Svelte with Auth</h1>

<div>
    {#if !userInfo}
        <a href="/login">Login</a>
    {/if}
    {#if userInfo}
        <a href="/logout"> Logout </a>
    {/if}
</div>

{#if userInfo}
    <h2 class="user">
        You are: {userInfo && userInfo.userDetails}
    </h2>
{/if}
{#if !userInfo}
    <h2>We don't know who you are?</h2>
{/if}
