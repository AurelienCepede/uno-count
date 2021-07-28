<script>
    export let players = ['Emilie', 'Aurélien', 'Jacky', 'Sylvie'];
    let rounds = {}
    players.forEach(player => rounds[player] = [])

    let ranking = []
    $: players = Object.keys(rounds)
    $: round_iterator = rounds[players[0]]
    const widthCol = "width : " + 100/Object.keys(rounds).length + "%"
    console.log(widthCol)
    const addRound = () => {
        if (ranking.length !== players.length) return false
        ranking.reverse().forEach((player, index) => {
            rounds[player].push(index)
        })
        rounds = rounds
        ranking = []
    }

    const rank = player => {
        const rank = ranking.indexOf(player) + 1
        const rank_string = `${rank}e${rank === 1 ? "r" : ""}`
        const points = players.length - rank
        const points_string = `${points} pt${points > 1 ? "s" : ""}`

        if (rank === 0) return ""

        return `${rank_string}<br>${points_string}`
    }

    const totalRank = player => {
        const total_points = players.map(p => rounds[p].reduce((total, round) => total + round, 0))
        return total_points[players.indexOf(player)]
    }

    const toggleRank = (player) => e => {
        if (ranking.indexOf(player) > -1)
            ranking = ranking.filter(p => p !== player)
        else
            ranking = [...ranking, player]

        rounds = rounds
    }


</script>
<style>
    table {
        width: 100%;
    }
    .selection {
        background-color: #73C2FB;
        color: white;
        border: none;
        border-radius: 0;
        margin-left: 5px;
        margin-right: 5px;
        max-width: 150px;
        width: 100%;
        height: 80px;
        vertical-align: middle;
    }
    .main {
        text-align:center;
        max-width:100%;
    }
</style>

<div class="main">

    <table>
        <thead>
        <tr>
            {#each players as player, index}
                <th style={widthCol}>
                    {player}<br>{totalRank(player)}
                </th>
            {/each}
        </tr>
        <tr>
            {#each players as player, index}
                <th>
                    <button class="selection" for={`select-player-${index}`} on:click={toggleRank(player)}>
                        {@html rank(player)}
                    </button>
                </th>
            {/each}
        </tr>

        <tr>
            <th  colspan={players.length}>
                <button on:click={addRound} disabled={ranking.length !== players.length}>
                    Valider
                </button>
            </th>
        </tr>
        </thead>
        {#each round_iterator as r, index}
            <tr>
                {#each players as player}
                    <td>{rounds[player][index]}</td>
                {/each}
            </tr>
        {/each}
    </table>
</div>