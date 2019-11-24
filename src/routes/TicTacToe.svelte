<script>
    let turn = 0;
    let null_value = "--";
    let winner = 0;

    function getInitialBoard() {
        let board = [];
        let fields = [0, 1, 2, 3, 4, 5, 6, 7, 8].map(k => {
            return { id: k, value: null_value };
        });
        while (fields.length > 0) {
            board.push(fields.splice(0, 3));
        }

        return board;
    }

    let board = getInitialBoard();

    function handleClick(field) {
        if (field.value == null_value && !winner) {
            field.value = turn ? "O" : "X";
            turn = turn ? 0 : 1;
            board = board;
            winner = Winner(board);
        }
    }
    function Reset() {
        board = getInitialBoard();
        winner = 0;
    }
    function Winner(board) {
        let fixedboard = [
            ...board,
            // Add diagonals
            [board[0][0], board[1][1], board[2][2]],
            [board[0][2], board[1][1], board[2][0]],
            // And verticals
            [board[0][0], board[1][0], board[2][0]],
            [board[0][1], board[1][1], board[2][1]],
            [board[0][2], board[1][2], board[2][2]]
        ];
        let row = [];
        for (row of fixedboard) {
            if (
                row[0].value === row[1].value &&
                row[0].value === row[2].value &&
                row[0].value !== null_value
            ) {
                return row[0].value === "X" ? "one" : "two";
            }
        }
        return 0;
    }
</script>

<style>
    span {
        display: inline-block;
    }
    div {
        text-align: center;
    }
</style>

<div>
    {#if winner}
        <h1>Player {winner} won! Reset to play again</h1>
    {:else}
        <h1>Player {turn ? 'two' : 'one'} to move</h1>
    {/if}

    {#each board as row}
        <span>
            {#each row as field}
                <p on:click={() => handleClick(field)}>
                    &nbsp {field.value} &nbsp
                </p>
            {/each}
        </span>
    {/each}
    <div>
        <button on:click={Reset}>Reset</button>
    </div>
</div>
