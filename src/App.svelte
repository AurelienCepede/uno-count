<script>
import Game from './Game.svelte'
let players = []
let ready = false

const addPlayer = () => {
    let nameFound = false
    while(!nameFound) {
        const name = prompt("Comment s'appelle le nouveau joueur : ", "Joueur " + (players.length + 1))
        if(name === null) return false
        if(players.indexOf(name) === -1) nameFound = name
    }
    players = [...players, nameFound]
}

const removePlayer = player => () => {
    players = players.filter(p => p !== player)
}

const toggleReady = () => ready = !ready
</script>

{#if players.length && ready}
    <button on:click={toggleReady}>Modifier les joueurs</button>
    <Game {players}/>
{:else}
    <button on:click={toggleReady}>Commencer la partie</button>
    <br><br>
    <button on:click={addPlayer}>Ajouter un joueur</button>
    {#each players as player}
        <div>
            <input type="text" bind:value={player}> <button on:click={removePlayer(player)}>Supprimer</button>
        </div>
    {/each}
{/if}



