<script>
		let rounds = {
			'Emi': [],
			'Auré': [],
			'Jacky': [],
			'Sylvie': []
    	}
		
		let ranking = []
		$: round_iterator = rounds['Emi']
		$: players = Object.keys(rounds)
		
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
			const points_string = `(${points} point${points > 1 ? "s" : ""})`
			
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
	.selection {
		background-color: #73C2FB;
		color: white;
		border: none;
		border-radius: 0;
		margin-left: 5px;
		margin-right: 5px;
		min-width: 150px;
		height: 80px;
		vertical-align: middle;
	}
	.center {
		text-align:center;
	}
</style>

<div class="center">
	
	<table>
		<thead>
		<tr>
			{#each players as player, index}
				<th>
					{player}<br>{totalRank(player)}
				</th>
			{/each}
		</tr>
		<tr>
			{#each players as player, index}
				<th>
					<button class="selection" for={`select-player-${index}`} on:click={toggleRank(player)}>
						{player}<br>{@html rank(player)}
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



