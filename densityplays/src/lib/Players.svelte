<script lang="ts">
    import { onMount } from "svelte";
    import { pb } from "./densityplays";
    import { Record } from "pocketbase";

    let userList = [];
    // let users = [];
    let usersCall = getUserActivityMaps();
    let username: string;
    let userRecord = Record;
    onMount(async () => {
        const userListR = await pb.collection('bnet_users').getList(1,100,{sort: "username"});
        userList = userListR.items
    });
    async function getUserActivityMaps() {
        const apiURL = pb.buildUrl(`/topkek/${username}`);
        const results = await fetch(apiURL);
        const results_body = await results.json();
        console.log(results_body)
        userRecord = await pb.collection('bnet_users').getFirstListItem(`username="${username}"`)
        return results_body["Users"]

    };
    function wrapGush(){
        usersCall = getUserActivityMaps()
    }

</script>

<form on:submit|preventDefault={wrapGush}>
    <select bind:value={username}>
        {#each userList as user}<option>{user.username}</option>{/each}
    </select>
    <button type="submit">Deez</button>
</form>

{#if username}
<div class="init_user">
    <div class="msg">
        <img class="avatar" src={`https://api.dicebear.com/6.x/bottts-neutral/svg?seed=${username}`} alt="avatar"width="40px"/>
        <div>
          <p class="msg-text">{username}</p>
        </div>
      </div>
</div>
{#await usersCall}
...loading...
{:then users} 
<div class="users">
    {#each users as user}
    <div class="user">
        <img
          class="avatar"
          src={`https://api.dicebear.com/6.x/identicon/svg?seed=${user.Name}`}
          alt="avatar"
          height=15em
        />
          {user.Name}
          <b>{user.TotalRaids}</b><br>
    <p class="raid-text">
        {#each user.Raids as raid}
        <img
        class="avatar"
        src={`https://avatars.dicebear.com/api/identicon/${raid.Name}.svg`}
        alt="avatar"
        height=15em
      />
        {raid.Name} <b>{raid.RaidCount}</b><br>
        {/each}
    </p>
      </div>
    {/each}
</div>
{/await}

{/if}