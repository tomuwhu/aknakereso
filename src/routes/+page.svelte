<script>
    const n = 15, asz = 30
    const t = Array(n ** 2 - asz).fill(" ").concat(Array(asz).fill("💣")).sort(() => Math.random() - 0.5)
    var eg = false
    var ta = Array(n).fill(0).map((v, i) => Array(n).fill(0).map((q, j) => t[n * i + j]))
    function f(x, y) {
        if (ta[x][y] == '💣') eg = true 
        else {
            let hvm = 0;
            [-1,0,1].forEach(vx => {
                [-1,0,1].forEach(vy => {
                    if (ta[x+vx] && ta[x+vx][y+vy] && ta[x+vx][y+vy] == "💣") hvm ++
                })
            })
            ta[x][y] = hvm
            if (hvm == 0) {
                [-1,0,1].forEach(vx => {
                    [-1,0,1].forEach(vy => {
                        if (ta[x+vx] && ta[x+vx][y+vy] && ta[x+vx][y+vy] == " " ) f(x + vx, y + vy)
                    })
                })
            }
        }
    }
    function g(x, y, e) {
        ta[x][y] = '📍'
        e.preventDefault()
    }
</script>
<h1>Aknakereső</h1>
<table>
    {#if !eg}
    {#each ta as row, i}
        <tr>
            {#each row as cell, j}
                <td on:click={()=>f(i, j)} 
                    on:contextmenu={e => g(i, j, e)} 
                    class={[0,1,2,3,4,5,6,7].includes(cell) ? "U" : (cell == '📍' ? "J" : "")}>{cell == '💣'?'':cell}</td>
            {/each}
        </tr>
    {/each}
    {:else}
    {#each ta as row, i}
    <tr>
        {#each row as cell, j}
            <td>{cell}</td>
        {/each}
    </tr>
    {/each}
    {/if}
</table>
<style>
    td.J {
        background-color: #14a8b6;
    }
    td.U {
        background-color: #44b8c6;
    }
    td {
        width: 30px;
        height: 30px;
        background-color: rgb(0, 139, 139);
        border-radius: 7px;
        cursor: pointer;
        color: aliceblue;
    }
    td:hover {
        background-color: rgb(204, 102, 0);
    }
    table {
        border-spacing: 6px;
    }
</style>
